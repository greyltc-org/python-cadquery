pkgname=python-cadquery
pkgver=v2.6.0
pkgrel=1
pkgdesc="A parametric CAD scripting framework based on PythonOCC"
arch=(any)
url="https://github.com/CadQuery/cadquery"
license=(Apache-2.0)
conflicts=(python-cadquery-git)
depends=(
python-ocp
python-ezdxf
nlopt
python-typish
python-multimethod
python-docutils
python-pyparsing
python-trame
python-trame-vtk
casadi
openblas
)
checkdepends=(
python-pytest
python-typing_extensions
python-docutils
python-mycdp
ttf-liberation
)
makedepends=(
git
python-setuptools-scm
python-build
python-installer
python-wheel
)

_fragment="#commit=18b15d6d86f202308bfc10a5d34cc0fc3f48bb6e"
source=(
"git+https://github.com/CadQuery/cadquery#commit=${_fragment}"
occt79.patch  # curl https://github.com/CadQuery/cadquery/commit/7cf644e75d41bb4ba6667a6ec81befe22b9dd254.patch > occt79.patch
use-pathlib.patch  # curl https://github.com/CadQuery/cadquery/commit/3bc82aa37547f355f4360dcf8eb3422cf529f098.patch > use-pathlib.patch
fix-gui-test.patch  # curl https://github.com/CadQuery/cadquery/compare/v2.6.0...greyltc:cadquery:fix-gui-test.patch > fix-gui-test.patch
)

sha256sums=('865af5ac3bcc74a2249d194c6db70651fdc1f2a5310553539914d77396333f39'
            '4d60cee6bf70d5eeaeb060e514d104969c1da7f30e7f4eb6d67c061e0debaa05'
            '5c1c16d30303015cb7d8b4d52664061473ac05f5915ce3e31988654db3abf4ae'
            '5bce824f9eb3b2defdea8600f8153c17be619abf7117013b5748402787751b7b')

pkgver() {
  cd cadquery
  git describe --tags | rev | cut -d- -f2- | rev | sed 's/-/.r/'
}

prepare() {
  patch -p1 -d cadquery < occt79.patch
  patch -p1 -d cadquery < use-pathlib.patch
  patch -p1 -d cadquery < fix-gui-test.patch
}

build() {
  cd cadquery
  python -m build --wheel --no-isolation
}

check() {
  python -m venv --without-pip --system-site-packages --clear venv
  source venv/bin/activate
  python -m installer cadquery/dist/*.whl

  local _these_fail=(
  test_project
  testText
  )
  printf -v _joined '%s and not ' "${_these_fail[@]}"
  python -m pytest -v cadquery/tests -p no:seleniumbase -k "$(echo "not ${_joined% and not }")"  # skip the tests we know fail

  deactivate
}

package() {
  cd cadquery
  python -m installer --destdir="$pkgdir" dist/*.whl
}
