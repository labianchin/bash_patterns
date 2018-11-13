## Bash patterns


Check these links:

- https://github.com/progrium/bashstyle
- https://www.shellcheck.net/
- https://kvz.io/blog/2013/11/21/bash-best-practices/
- http://www.kfirlavi.com/blog/2012/11/14/defensive-bash-programming/
- http://wiki.bash-hackers.org/scripting/obsolete
- https://google.github.io/styleguide/shell.xml


### Examples

```bash
#!/usr/bin/env bash

set -o errexit
set -o nounset
set -o pipefail

readonly DIR="$( cd "$( dirname "$0" )" && pwd )"
readonly RANDOM_ID=$(head -c 24 /dev/urandom | base64)  # 33 chars
[[ "$TRACE" ]] && set -o xtrace

main() {
  if [[ $# -gt 0 ]]; then
    "$@"
  else
    default_cmd
  fi
  echo "DONE!"
}

[[ "$0" == "$BASH_SOURCE" ]] && time main "$@"

clean_value() {
  local v=${1,,}  # lowercase
  v="${v//[!a-z0-9-]/-}"  # only a-z0-9-
  echo "${v:0:63}"  # 63 max chars
}

URI_NO_SLASH_AT_END=${URI%%/}

cat <(echo "123")
base64 <(echo "123")  # echo "123" | base64

cat <(base64 --decode << EOF
MTIzCg==
EOF
)

is_osx() {
    [[ $('uname') == 'Darwin' ]]
}

is_linux() {
    [[ $('uname') == 'Linux' ]]
}

adddate() {
  # adds data to output: e.g. somecommand | adddate
  while IFS= read -r line; do
    echo "$(date +"[%F %T]") $line"
  done
}

if ! hash something 2>/dev/null; then
    echo "'something' was not found"
fi

# list symlinks
find "$HOME" -maxdepth 2 -type l -exec ls -lah --color {} + 2>/dev/null | sed -e 's/.* \(.* -> .*\)/\1/'


# add line if not exists
grep -Fq 'source ~/.myterminalrc' ~/.bashrc || echo 'source ~/.myterminalrc' >> ~/.bashrc

# skip N lines
tail -n +2 /tmp/csv_file_with_header.csv


# heredocs
# https://stackoverflow.com/questions/1167746/how-to-assign-a-heredoc-value-to-a-variable-in-bash
# https://stackoverflow.com/questions/4937792/using-variables-inside-a-bash-heredoc
VAR=$(cat <<'EOF'
something'bla"
$(dont-execute-this)
foo"bar"'
EOF
)

VAR=$(cat <<EOF
something'bla"
$(do-execute-this)
foo"bar"''
EOF
)

# for each argument run command, in parallel
printf '%s\n' "$@" | xargs -I{} -P4 -t echo foo {} bar

```
