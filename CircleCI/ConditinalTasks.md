# Create ENV vars for complex conditions and use in task execution

``` yaml
  environment:
    # The build number is auto-incremented by CircleCI with each build
    VER: 0:0:$CIRCLE_BUILD_NUM
    TAG: build-$CIRCLE_BRANCH-$CIRCLE_BUILD_NUM
    SKIP_EMULATOR: git log --format=oneline -n 1 $CIRCLE_SHA1 | grep skip-emulator-test
    ALGO_COMMIT: echo "Tzuri mdaragentvi d.gilon ellap" | grep $(git log --pretty=format:%cN -n 1 $CIRCLE_SHA1)

test:
  override:
    - if [[ ! $SKIP_EMULATOR  && ( ! -z $ALGO_COMMIT || "$CIRCLE_BRANCH" = "dev" || "$CIRCLE_BRANCH" =~ "rc-") ]]; then sh trigger_as_emulator.sh; fi

```