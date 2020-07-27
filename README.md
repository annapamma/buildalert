# buildalert Orb [![CircleCI Build Status](https://circleci.com/gh/annapamma/buildalert.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/annapamma/buildalert) [![CircleCI Orb Version](https://img.shields.io/badge/endpoint.svg?url=https://badges.circleci.io/orb/oss-tools/buildalert)](https://circleci.com/orbs/registry/orb/oss-tools/buildalert) [![GitHub License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](https://raw.githubusercontent.com/annapamma/buildalert/master/LICENSE) [![CircleCI Community](https://img.shields.io/badge/community-CircleCI%20Discuss-343434.svg)](https://discuss.circleci.com/c/ecosystem/orbs)

Utilize this orb to easily add CircleCI build monitoring to your CI/CD pipeline. 
This orb allows you to set up Slack notifications if user has triggered too many builds in a certain
amount of time, or if there have been too many builds triggered across the organization. 


## Usage

The best way to use this is with a private repository to monitor your OSS repository. 
For example, if you would like to monitor a repo named `open_source_project`, you can set up
another repo with the following config:

```yaml
version: 2.1

orbs:
  buildalert: oss-tools/buildalert@0.0.4

workflows:
  monitor-every-minute:
    triggers:
      - schedule:
          cron: "* * * * *"
          filters:
            branches:
              only:
                - master
    jobs:
      - buildalert/monitor-builds:
          context: 'build_alert'
          circle-project-org: 'my-org-name'
          circle-project-reponame: 'open_source_project'
          circle-token-envvar: 'SLACK_MONITOR_CIRCLE_TOKEN'
          slack-app-url-envvar: 'SLACK_MONITOR_SLACK_APP_URL'
          gh-token-envvar: 'SLACK_MONITOR_GH_TOKEN'
          alert-threshold-seconds: 60
          alert-threshold-max-builds-per-user: 5
          alert-threshold-max-builds: 10
  monitor-siren-test:
    triggers:
      - schedule:
          cron: "0 0 1 * *"
          filters:
            branches:
              only:
                - master
    jobs:
      - buildalert/monitor-builds:
          context: 'build_alert'
          circle-project-org: 'my-org-name'
          circle-project-reponame: 'open_source_project'
          circle-token-envvar: 'SLACK_MONITOR_CIRCLE_TOKEN'
          slack-app-url-envvar: 'SLACK_MONITOR_SLACK_APP_URL'
          gh-token-envvar: 'SLACK_MONITOR_GH_TOKEN'
          alert-threshold-max-builds-per-user: 0
          alert-threshold-max-builds: 0
          siren-test: true
```

This config has two [scheduled workflows](https://circleci.com/docs/2.0/workflows/): `monitor-every-minute` and `monitor-siren-test`. 

The `monitor-every-minute` workflow has been configured to run every minute. This monitor is set up to alert 
Slack if there have been more than 5 pipelines triggered by any given user or more than 10 pipelines for the whole 
repository in the past 60 seconds.

The `monitor-siren-test` workflow has been configured at midnight on the first day of every month. 
The alert thresholds have been set to 0 to force a trigger, and `siren-test` has been set to `true` so that alerts will be prefaced with the text "THIS IS JUST A TEST".

Example use-cases are provided on the orb [registry page](https://circleci.com/orbs/registry/orb/oss-tools/buildalert#usage-examples). Source for these examples can be found within the `src/examples` directory.


## Resources

[CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/oss-tools/buildalert) - The official registry page of this orb for all versions, executors, commands, and jobs described.  
[CircleCI Orb Docs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) - Docs for using and creating CircleCI Orbs.  

### How To Contribute

We welcome [issues](https://github.com/annapamma/buildalert/issues) to and [pull requests](https://github.com/annapamma/buildalert/pulls) against this repository!

For further questions/comments about this or other orbs, visit the Orb Category of [CircleCI Discuss](https://discuss.circleci.com/c/orbs).
