# FontForge에 기여하기

## 문제 보고

[FontForge 자체에 문제가 발생했을 때](http://designwithfontforge.com/en-US/When_Things_Go_Wrong_With_Fontforge_Itself.html)를 참조하십시오.

## 코드 기여

마스터 브랜치에 병합하려면 프로젝트의 활성 핵심 기여자의 풀 리퀘스트 검토 및 승인이 필요합니다. 검토자는 수정되는 코드를 완전히 이해해야 하며, 모든 변경 사항은 메일링 리스트에서 사전 논의되지 않는 한 엄격하게 비회귀적이고 비파괴적이어야 합니다. FontForge는 매우 복잡한 소프트웨어이며, 최상의 상태를 유지하려면 이 수준의 주의가 필요합니다. 대규모 풀 리퀘스트는 코드 검토 리소스가 제한되어 있기 때문에 잘 작성되었더라도 승인하는 데 오랜 시간이 걸리거나 거부될 수 있다는 점을 이해해 주십시오.

### 기여 방법, 단계별 안내

GitHub의 풀 리퀘스트를 사용하여 코드베이스에 직접 기여하십시오.
자세한 내용은 [Github 가이드](https://guides.github.com/)를 참조하십시오. 기본 프로세스는 다음과 같습니다.

- GitHub에서 [FontForge 저장소](https://github.com/fontforge/fontforge)를 포크합니다.
- `git`을 사용하여 로컬에서 변경 사항을 커밋하고 개인 포크에 푸시합니다.
- 포크의 기본 페이지에서 녹색 "포크" 버튼을 클릭하여 풀 리퀘스트를 제출합니다.
- 풀 리퀘스트는 [GitHub Actions](https://github.com/features/actions)를 통해 테스트되어 변경 사항이 컴파일을 방해하지 않는지 자동으로 표시합니다. FontForge는 큰 프로그램이므로 변경 사항이 빌드 가능한지 확인하는 데 20분 이상 걸릴 수 있습니다. 잠시만 기다려 주십시오. CI에 대한 자세한 내용은 아래에 있습니다.
- 문제가 있다고 보고되면 "세부 정보" 링크를 따라 풀 리퀘스트에 대한 로그 보고서를 확인하여 문제가 무엇인지 확인할 수 있습니다.

FontForge는 Python `>=` 3.3을 지원하며 최소 3.9 버전까지 Python 3과 완벽하게 호환됩니다.

### 코딩 스타일

이러한 가이드라인 중 일부는 저장소의 가장 오래된 코드에서는 따르지 않지만, FontForge가 1인 천재 프로젝트에서 협업 커뮤니티 프로젝트로 전환된 2012년 이후의 모든 새 코드에 사용하고자 합니다.

1. 한 줄에 하나의 문장을 사용하여 반자동 처리 및 diff 읽기를 훨씬 쉽게 만듭니다.
2. 부울 변수는 정수가 아닌 `stdbool.h`의 이름 `true`와 `false`를 사용해야 합니다([참조](https://github.com/fontforge/fontforge/issues/724)).
3. `return` 문은 배치되는 들여쓰기 수준과 일치해야 합니다. 기존 코드의 대부분처럼 왼쪽 여백에 두지 마십시오([참조](https://github.com/fontforge/fontforge/issues/1208)).

이러한 가이드라인 외에도 수정 중인 파일에 사용된 스타일을 따르도록 노력하십시오.

### 문의할 사람

최근 몇 년 동안 코드베이스의 다양한 영역은 여러 사람이 작업했으므로 작업 중인 일반적인 영역에 익숙하지 않은 경우 해당 영역에 경험이 있는 사람들과 자유롭게 대화하십시오.

* 빌드 시스템: Debian - Frank Trampe (frank-trampe)
* 빌드 시스템: OS X (응용 프로그램 번들, Homebrew) - Jeremy Tan (jtanx)
* 빌드 시스템: Windows - Jeremy Tan (jtanx)
* 기능: UFO 가져오기/내보내기 - Frank Trampe (frank-trampe)
* 기능: Python 인터페이스 - Skef Iterum (skef)
* 충돌: Frank Trampe, Adrien Tetar (adrientetar)

### CI 빌드 아카이브 액세스

각 푸시 요청 후 `Appveyor`는 Windows 설치 프로그램을 빌드하고 패키징하려고 시도합니다. 해당 빌드가 성공하면 `Appveyor` "세부 정보" 링크를 따라 "아티팩트" 탭을 선택하여 액세스할 수 있습니다.

참고: `Appveyor`는 다양한 초기화 및 구성 검색 경로를 변경하는 `FF_PORTABLE` 플래그로 빌드합니다.

Github Actions를 통한 CI는 Mac OS X 번들 및 Linux AppImage도 빌드합니다. 해당 빌드가 성공하면 다음에 따라 다운로드할 수 있습니다.

    https://docs.github.com/en/actions/managing-workflow-runs/downloading-workflow-artifacts

## FontForge 번역
번역 처리를 위해 Crowdin 사용을 시험하고 있습니다. FontForge 번역에 기여하고 싶다면 여기에서 하십시오: https://crowdin.com/project/fontforge

이는 FontForge의 다음 릴리스에 포함될 것입니다.

## 패키지 빌드

소스에서 FontForge를 빌드하는 방법에 대한 정보는 [INSTALL.md](INSTALL.md)를 참조하십시오.

### 추가 파일

CID 키 글꼴을 편집하려면 [`/contrib/cidmap`](https://github.com/fontforge/fontforge/tree/master/contrib/cidmap)에 문자 집합 설명이 필요합니다.

일부 오래된 유니코드 비트맵 글꼴을 가져오고 싶을 수 있습니다.

-   [unifont](http.czyborra.com/unifont/)
-   [FreeFont 프로젝트](http://www.nongnu.org/freefont/)
-   [X fixed](http://www.cl.cam.ac.uk/~mgk25/ucs-fonts.html)
-   [Computer Modern 유니코드 글꼴](http://canopus.iacp.dvo.ru/~panov/cm-unicode/) - [자유/리브레 오픈 소스 운영 체제를 위한 유니코드 글꼴 가이드](http://eyegene.ophthy.med.umich.edu/unicode/fontguide/)

### Debian 소스 패키지 빌드

Debian 소스 패키지는 소스 tarball(특정 메타데이터 포함)과 여러 첨부 파일로 구성되며 중립적인 빌드 환경에서 제품을 빌드할 수 있습니다.

소스 패키지는 대상 배포판(아키텍처가 아님)에 따라 다릅니다.
가장 일반적인 빌드 대상은 현재 Launchpad 빌드 지원이 포함된 장기 지원 릴리스인 Ubuntu Xenial입니다. Xenial용으로 빌드된 바이너리 패키지는 일반적으로 이후 버전의 Ubuntu에도 설치 및 실행됩니다.

첫 번째 단계는 배포판 tarball을 얻는 것입니다. git 소스에서 이를 생성하려면 소스 디렉터리로 변경하고 다음을 실행하십시오.

```bash
mkdir build && cd build
cmake ..
make dist
```

그러면 `fontforge-20190801.tar.xz`와 유사한 이름의 아카이브가 생성됩니다. 이것을 원하는 작업 폴더로 이동/추출하고 `Packaging/debian/setup-metadata.sh`를 실행하십시오.

```bash
tar axf fontforge-20190801.tar.xz
cd fontforge-20190801
Packaging/debian/setup-metadata.sh
```

그러면 필요한 메타데이터가 최상위 `debian` 폴더에 복사됩니다. 원하는 경우 `debian/changelog`를 생성하라는 메시지도 표시됩니다. 이 상태에서 Debian 소스 패키지는 다음을 실행하여 생성할 수 있습니다.

```bash
debuild -S -sa
```

성공적으로 완료되면 소스 패키지 빌드는 부모 디렉터리에 다음과 같은 이름의 여러 파일을 남깁니다.

    fontforge-20190801-0ubuntu1~xenial.dsc
    fontforge-20190801-0ubuntu1~xenial_source.build
    fontforge-20190801-0ubuntu1~xenial_source.changes
    fontforge-20190801-0ubuntu1~xenial.tar.gz

빌드를 위해 Launchpad 저장소에 업로드하려면 다음과 같이 대상 저장소를 첫 번째 인수로 사용하여 `.changes` 파일에서 dput을 실행할 수 있습니다.

    dput ppa:fontforge/fontforge fontforge-20190801-0ubuntu1~xenial_source.changes

성공하면 `fontforge-20190801-0ubuntu1~xenial_source.ppa.upload`와 같은 이름의 파일이 남아 중복 업로드를 차단합니다.

업로드된 패키지의 유효성이 검사되면 Launchpad는 지원되는 모든 아키텍처에 대해 패키지를 빌드합니다.
그런 다음 Launchpad 웹 인터페이스를 통해 Xenial에서 다른 Ubuntu 버전으로 바이너리 패키지를 복사할 수 있습니다.

Launchpad에 대한 자세한 내용은 [여기](https://help.launchpad.net/Packaging/PPA)를 참조하십시오.

소스 패키지에서 로컬로 바이너리 패키지를 빌드할 수도 있습니다.
`make deb-src`에서 생성된 `tar.gz` 파일을 새 디렉터리에 추출하고 디렉터리로 들어가 `debuild`를 실행하면 됩니다.

### Red Hat 소스 패키지(.rpm) 빌드

spec 파일을 얻으려면 다음을 실행하십시오.

```
Packaging/redhat/generate-spec.sh
```

그러면 `FontForge.spec` 파일이 생성됩니다.

로컬에서 바이너리 패키지를 빌드하려면 배포판 아카이브(Debian 지침에 따라)를 `~/rpmbuild/SOURCES`에 복사하고 spec 파일을 `~/rpmbuild/SPECS`에 복사하십시오. 그런 다음 `rpmbuild -ba ~/rpmbuild/SPECS/FontForge.spec`을 실행하십시오.
예:

```
mkdir -p ~/rpmbuild/SOURCES/
cp TARBALL ~/rpmbuild/SOURCES/
rpmbuild -ba --nodeps SPECFILE
```

성공하면 `~/rpmbuild/RPMS`에 바이너리 패키지가, `~/rpmbuild/SRPMS`에 소스 패키지가 남습니다.

일반적으로 Fedora 파생 시스템용으로 패키징된 종속성을 설치해야 할 수 있습니다.

    rpm-devel rpm-build git ninja-build cmake gcc g++ python3-devel libjpeg-devel libtiff-devel libpng-devel giflib-devel freetype-devel libxml2-devel libspiro-devel pango-devel cairo-devel gtk3-devel woff2-devel

### Mac OS X 앱 번들 빌드

앱 번들을 빌드하는 대상이 있습니다.

```sh
make macbundle
```

그러면 빌드 디렉터리의 `osx` 하위 디렉터리에 `FontForge.app`이 생성됩니다.

이는 번들을 만드는 [`ffosxbuild.sh`](.github/workflows/scripts/ffosxbuild.sh)에 의존합니다. Homebrew 및 GDK 백엔드에서 작동하는 것으로 테스트되었습니다.
