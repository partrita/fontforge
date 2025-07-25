# 소스에서 FontForge 설치하기

일반적인 모든 빌드 도구, 빌드 종속성 및 런타임 종속성, 즉 소스 코드에서 소프트웨어를 빌드할 수 있는 모든 패키지를 설치하십시오.
이를 수행하는 정확한 방법은 OS 배포판에 따라 다릅니다.

Ubuntu에서 모든 종속성을 다운로드하려면 다음을 실행하십시오.

```sh
sudo apt-get install libjpeg-dev libtiff5-dev libpng-dev libfreetype-dev libgif-dev libgtk-3-dev libxml2-dev libpango1.0-dev libcairo2-dev libspiro-dev libwoff-dev python3-dev ninja-build cmake build-essential gettext;
```

이제 빌드 및 설치 스크립트를 실행하십시오.

```sh
cd fontforge
mkdir build
cd build
cmake -GNinja ..
ninja
sudo ninja install
```

빌드 시스템 자체에 대한 자세한 내용은 [이 위키 페이지](https://github.com/fontforge/fontforge/wiki/CMake-guide-for-FontForge)를 참조하십시오.

**TrueType 힌팅을 좋아하는 디자이너 주목:**
`-DENABLE_FREETYPE_DEBUGGER=/path/to/freetype/source`로 `cmake`를 실행하십시오.
이 옵션은 힌팅 지침을 하나씩 단계별로 실행하는 것과 같이 TrueType 글꼴 힌트를 디버깅하기 위한 고급 기능을 활성화합니다.

FreeType 소스의 버전은 설치된 FreeType 버전과 *정확히* 일치해야 합니다.

## 일반적인 문제
### MacOS
* 누락된 패키지는 Homebrew로 설치할 수 있습니다. 예:
```
brew install cmake glib pango gtk+3
```
* `msgfmt`를 찾을 수 없음(Homebrew 사용 시):

   ```bash
   brew install gettext
   brew link gettext
   ```

#### 실행 파일

문자 이미지 주위에 자동 추적을 하려면 다음 중 하나를 다운로드해야 합니다.

- Peter Selinger의 [potrace](http.potrace.sf.net/)
- Martin Weber의 [autotrace 프로그램](http://sourceforge.net/projects/autotrace/)

#### 라이브러리

시스템에 패키지 관리자가 있는 경우 사용하십시오.
이러한 라이브러리를 더 쉽게 설치할 수 있습니다.

이들 대부분은 FontForge의 적절한 컴파일/실행에 필요하지 않으며, 라이브러리가 없으면 사용되지 않습니다.
(실행 파일이 빌드된 컴퓨터에 라이브러리가 없는 경우 라이브러리를 설치할 뿐만 아니라 **소스에서 FontForge를 다시 빌드해야 합니다**.
컴퓨터에 없고 원하는 경우 다음에서 구할 수 있습니다.

-   이미지 라이브러리 (FontForge가 자동 추적을 위한 배경으로 일반적으로 사용되는 형식의 이미지를 가져올 수 있도록 함)
    -   [libpng](http://www.libpng.org/pub/png/libpng.html) (및 필수 도우미 [zlib](http://www.zlib.net/))
    -   [libtiff](http://www.libtiff.org/)
    -   [libungif](http.gnuwin32.sourceforge.net/packages/libungif.htm)
    -   [libjpeg](http://www.ijg.org/)
-   [libxml2](http://xmlsoft.org/) SVG 파일 및 글꼴 구문 분석
-   [libspiro](https://github.com/fontforge/libspiro) Raph Levien의 클로소이드를 베지어 스플라인으로 변환하는 루틴입니다. 이것이 있으면 FontForge에서 클로소이드 스플라인(spiro)으로 편집할 수 있습니다.
-   [libiconv](http://www.gnu.org/software/libiconv/) 기본 제공 iconv()가 없는 시스템에만 중요합니다.
    없는 경우 FontForge에는 작동할 수 있는 최소 버전의 라이브러리가 포함되어 있습니다.
    그러나 libiconv를 사용하려면 FontForge에 Shift-JIS가 필요하므로 `--enable-extra-encodings`로 구성해야 합니다.
-   [freetype](http://freetype.org/)
    비트맵을 더 잘 래스터화하고 트루타입 디버거를 활성화합니다.
    FontForge의 일부 명령은 바이트 코드 인터프리터가 활성화된 상태에서 freetype을 컴파일하는 데 의존합니다.
    [Apple에 부여된 일부 특허](http://freetype.org/patents.html) 때문에 기본적으로 비활성화되어 있었습니다.
    만료되었으므로 설정에서 오래된 라이브러리 버전을 사용하는 경우가 아니면 더 이상 걱할 필요가 없습니다.
    그런 다음 라이브러리를 빌드하기 전에 `*.../include/freetype/config/ftoption.h*`에서 적절한 매크로를 설정하여 인터프리터를 활성화할 수 있습니다(freetype 배포판의 최상위 수준에 있는 README.UNX 파일 참조).
    트루타입 디버거를 활성화하려면 FontForge를 빌드할 때 freetype 소스 디렉터리를 사용할 수 있어야 합니다(여기에 의존하는 일부 '#include' 파일이 있음).
-   libintl 대부분의 Unix에서 표준입니다. Mac의 fink 패키지의 일부입니다. UI 현지화를 처리합니다.
-   [libpython](http.python.org/) FontForge가 컴파일될 때 있으면 사용자가 FontForge 내에서 파이썬 스크립트를 실행할 수 있습니다(그리고 FontForge의 기능을 파이썬으로 가져올 수 있도록 FontForge를 구성할 수 있습니다. 즉, FontForge는 파이썬을 *확장*하고 *내장*합니다).
-   [libX](http://x.org/) 일반적으로 FontForge는 X11 창 시스템에 의존하지만 스크립팅 엔진(사용자 인터페이스 없음)에만 관심이 있는 경우 X가 없는 시스템에서 빌드할 수 있습니다(구성 스크립트가 이를 파악해야 함).
-   [libcairo](http.cairographics.org/) Cairo는 외곽선 글리프 보기에서 앤티앨리어싱된 스플라인을 그립니다. libfontconfig, libXft 및 기타 라이브러리에 의존합니다.
-   [libpango](http://www.pango.org/) Pango는 복잡한 스크립트에 대한 텍스트를 그립니다. glib-2.0, libfontconfig, libfreetype, libXft 및 기타 라이브러리에 의존합니다.
-   [Unifont](http.savannah.gnu.org/projects/unifont)는 모든 유니코드 코드포인트에 대한 글리프를 포함하며, 설치된 경우 FontForge에서 사용합니다.
