# CircleCI Orbs

> Re-usable [CircleCI Orbs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) for common tasks.

## AWS Environment

```yaml
# .circleci/config.yml

orbs:
  aws: sbstjn/aws@0.1.0

workflows:
  version: 2

  stable:
    jobs:
      - checkout
      - aws/exec:
          name: deploy STABLE

          AWSAccessKey: $STABLE_ACCESS
          AWSSecretKey: $STABLE_SECRET
          command: ENV=stable make update

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

          AWSAccessKey: $PROD_ACCESS
          AWSSecretKey: $PROD_SECRET
          command: ENV=prod make update

          requires: [ checkout ]
          filters:
            tags:
              only: /v[0-9]+(\.[0-9]+)*/
            branches:
              ignore: /.*/
```

