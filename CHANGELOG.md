## v0.3.6+ rberger / fishnix
- volumes: actually uses options[:mount_options]
- volumes: can now specify the volume_type so gp2 can be supported
-- Made it backwards compatible if you don't specify volume_type and you specify iops it will set options[:volume_type] to io1
- Updated README to show the changes
- Added the .gitignore from fishnix

## v0.3.5
- Refactored code and added dependency on delayed_evaluator to fix a bug that
  prevented the proper device name from being used which was introduced with the
  uselvm flag.
## v0.3.4
- Merge upstream changes and update README
## v0.3.3
- Update README.md with example for persistent_volumes
## v0.3.2
- support for raid without using lvm
- support for persistent raid volumes with ebs volumes defined in attribtutes
## v0.3.1
- Fixed wrong starting value for mount when no volumes initially exist
- Moved to standard 3 digit cookbook version
## v0.0.3
- Initial release to Opscode cookbooks site.
