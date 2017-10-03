# Complext Logicall expression

## simple AND pluse many OR conditions
``` sh
- if [[ ! $SKIP_EMULATOR  && ( ! -z $ALGO_COMMIT || "$CIRCLE_BRANCH" = "dev" || "$CIRCLE_BRANCH" =~ "rc-") ]]; then sh trigger_as_emulator.sh; fi
```