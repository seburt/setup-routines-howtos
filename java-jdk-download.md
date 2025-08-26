# Oracle JDK download helper (Linux/macOS/Windows binaries)

You must accept the Oracle JDK License Update before downloading: https://www.oracle.com/java/technologies/javase-downloads.html

This script attempts to fetch the direct Oracle JDK download URL for a given version/platform/extension and download it with the license cookie.

```bash
# Usage: get_oracle_jdk_x64.sh <jdk_version> <platform> <ext>
# jdk_version: e.g., 14
# platform: linux | osx | windows
# ext: rpm | dmg | tar.gz | exe

jdk_version=${1:-14}
platform=${2:-linux}
ext=${3:-rpm}

readonly url="https://www.oracle.com"
readonly jdk_download_url1="$url/java/technologies/javase-downloads.html"
readonly jdk_download_url2=$(\
  curl -s $jdk_download_url1 | \
  egrep -o "/\\java\\/technologies\\/javase-jdk${jdk_version}-downloads.html"
)

[[ -z "$jdk_download_url2" ]] && echo "Could not get jdk download url - $jdk_download_url1" >> /dev/stderr

readonly jdk_download_url3="${url}${jdk_download_url2}"
readonly jdk_download_url4=$(\
  curl -s $jdk_download_url3 | \
  egrep -o "download.oracle\.com/otn-pub/java/jdk/(${jdk_version})\..*/.*/jdk-${jdk_version}\..*[_]${platform}-x64_bin.${ext}'" | tr -d '\''
)

for dl_url in ${jdk_download_url4[@]}; do
  wget --no-cookies \
       --no-check-certificate \
       --header "Cookie: oraclelicense=accept-securebackup-cookie" \
       -N $dl_url
done
```

After download, set JAVA_HOME and update PATH. For example:
```bash
# Append to ~/.bashrc or ~/.profile (adjust the path you found):
export JAVA_HOME="/path/to/jdk"
export PATH="$JAVA_HOME/bin:$PATH"
```

Notes:
- Oracle frequently changes download pages/flows; this approach may break over time. Consider using OpenJDK builds (Adoptium/Temurin, Azul Zulu) for easier scripted installs.
