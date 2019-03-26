# CircleCI Orbs

> Re-usable [CircleCI Orbs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) for common tasks.

## AWS Environment

```yaml
# .circleci/config.yml

orbs:
  aws: sbstjn/aws@0.2.0

workflows:
  version: 2

  stable:
    jobs:
      - checkout
      - aws/exec:
          name: deploy STABLE

          AccessKey: $STABLE_ACCESS
          SecretKey: $STABLE_SECRET
          Command: ENV=stable make update

          requires: [ checkout ]
          filters:
            branches:
              only: master

  release:
    jobs:
      - checkout:
          filters:
            tags:
              only: /v[0-9]+(\.[0-9]+)*/
            branches:
              ignore: /.*/
      - aws/exec:
          name: deploy PROD

          AccessKey: $PROD_ACCESS
          SecretKey: $PROD_SECRET
          Command: ENV=prod make update

          requires: [ checkout ]
          filters:
            tags:
              only: /v[0-9]+(\.[0-9]+)*/
            branches:
              ignore: /.*/
```

