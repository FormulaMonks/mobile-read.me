# Writing Changelogs

Keeping a `changelog.md` file updated is useful for many reasons:
- You'll need to update your _"What's new"_ entry on the [AppStore](https://developer.apple.com/app-store/product-page/) / Google Play.
- QA needs to know recent changes to review your latest build.
- Even you as a developer might want to review the past roadmap of your app.

So we've got at least 3 different sources asking for a similar information (if not the same):
- **Consumers:** AppStore / Google Play
- **QA:** Crashlytics / HockeyApp / TestFlight
- **Developers:** [Github Releases](https://blog.github.com/2013-07-02-release-your-software/)

My advice is to write once and distribute to many sources as possible.

AppStore might have a different copy because it has a completely different audience but this changelog file will be great as a starting point.

## How a `changelog.md` file looks like
```
# 1.1.0
Here's a small text as a summary of the items below. This might be useful for the App Store "What's new" entry.
- Added feature_A.
- Fixed crash.

# 1.0.0
- Initial release.
```

The only important thing here is that versions are in markup titles. We use that later when parsing the changelog and distribute it using a fastlane command.

# Automating Changelogs using Fastlane
For this you'll first need to implement these two _private lanes_ to fetch `version` and `build number` from your project.

Here's an implementation for **iOS**:
```ruby
  # gets build number from iOS Project's info.plist
  private_lane :get_build_number_from_plist do
    # this is required because default action "get_build_number" has some issues
    %x[/usr/libexec/PlistBuddy -c "Print CFBundleVersion" "../Project/Info.plist"].strip
  end

  # gets version from iOS Project's info.plist
  private_lane :get_version_from_plist do
    %x[/usr/libexec/PlistBuddy -c "Print CFBundleShortVersionString" "../Project/Info.plist"].strip
  end
```

Then you can generate changelogs from the `changelog.md` using this private lane:
```ruby
  # returns changelog from current version or "_No changelog found_" if
  # found none.
  private_lane :chlog do
    file = File.open('../changelog.md', 'r')
    data = file.read
    tmp = {}
    data.split('#').each do |i|
      version = i.lines.first.to_s.strip
      next if version == ''

      tmp[version] = i.lines[1..-1].join
    end

    comments = tmp[get_version_from_plist]
    comments.nil? ? '_No changelog found_' : comments
  end
```

Finally, you can use that string to attach it on your builds using Crashlytics / Hockey / etc or you can use it to create a **Github Release**:
```ruby
  # asumming tags are major.minor.patch.build_number
  private_lane :get_tag do
    "#{get_version_from_plist}.#{get_build_number_from_plist}"
  end

  lane :release_notes do
    current_tag = get_tag
    if !git_tag_exists(tag: current_tag)
      puts 'That tag does not exist'
      return
    end

    notes = chlog
    if notes.nil?
      puts 'No release notes'
      return
    end

    set_github_release(
      repository_name: "theorem/project",
      api_token: ENV["GITHUB_TOKEN"],
      name: "#{get_version_from_plist}",
      tag_name: current_tag,
      description: notes,
      commitish: "main",
      upload_assets: nil
    )
  end

  lane :tag_and_release do
    add_git_tag(tag: get_tag)
    push_git_tags
    release_notes
  end
```

With these last lanes you can input `fastlane tag_and_release` on your terminal and it will:
- Generate a tag as `major.minor.patch.build_number`
- Push that tag on Github
- Create a **Release Note** with what you wrote at `changelog.md` and associate that with this new tag.
