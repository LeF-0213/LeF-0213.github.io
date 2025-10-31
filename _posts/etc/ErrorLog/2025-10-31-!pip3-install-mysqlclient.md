# Xcode Command Line Tools (처음 한 번)
xcode-select --install
# Homebrew 설치되어 있다고 가정
brew install pkg-config mariadb-connector-c
# PKG_CONFIG_PATH 등록 (Apple Silicon 은 /opt/homebrew, Intel 은 /usr/local)
export PKG_CONFIG_PATH="$(brew --prefix)/opt/mariadb-connector-c/lib/pkgconfig"
# 가상환경 활성화 후
python -m pip install --upgrade pip wheel setuptools
pip install mysqlclient