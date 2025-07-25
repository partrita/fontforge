.. _PfaEdit-TeX.TeX:

FontForge와 TeX
=================

FontForge에는 TeX를 처리하기 위한 여러 기능이 내장되어 있습니다.

"pk" 및 "gf" 비트맵 파일을 읽고 :ref:`자동 추적 <elementmenu.AutoTrace>`하여 외곽선 글꼴을 생성할 수 있습니다. ".mf" 파일에서 직접 메타폰트를 호출하여 "gf" 비트맵을 생성하고 자동 추적하여 외곽선 글꼴을 생성할 수도 있습니다.

".tfm" 파일에서 합자, 커닝 정보를 읽을 수 있습니다. tfm 파일을 만들 수 있습니다.

`인코딩 <https://fontforge.org/downloads/Encodings.ps.gz>`__ 파일에는 여러 표준 TeX 인코딩이 내장되어 있습니다.

큰 CJK 트루타입 글꼴의 경우 ttf2tfm 매뉴얼 페이지에 정의된 하위 글꼴 정의 파일을 읽고 :doc:`생성 </ui/dialogs/generate>`하여 이를 기반으로 한 일련의 포스트스크립트 type-1 글꼴을 생성할 수 있습니다.

lilypond 그룹과 협의하여 TeX 특정 정보를 True/OpenType 파일에 저장하기 위한 새로운 :doc:`SFNT 테이블 'TeX <non-standard>` '을 설계했습니다.


tfm 파일(및 enc 파일) 생성.
----------------------------------------

tfm 파일을 생성하기 전에 다음 중 일부를 수행해야 합니다.

.. object:: 글꼴 매개변수 설정

   :menuselection:`요소 --> 글꼴 정보 --> TeX`를 사용하여 이 작업을 수행합니다.

.. _PfaEdit-TeX.Italic:

.. object:: 선택적으로 글리프에 대한 이탤릭 보정 값 설정

   (FontForge는 이탤릭 글꼴에 대한 기본값을 생성하므로 FontForge가 잘못된 경우에만 이 작업을 수행하면 됩니다).

   * 이탤릭 보정이 있어야 하는 글리프를 선택합니다.
   * :menuselection:`요소 --> 글리프 정보 --> 대체 위치`
   * [새로 만들기]를 누릅니다.
   * 풀다운 목록에서 태그를 ``이탤릭 보정``으로 설정합니다.
   * XAdvance를 이탤릭 보정으로 설정합니다.

.. _PfaEdit-TeX.charlist:

.. object:: 선택적으로 모든 글리프 목록 설정

   * 글리프 목록의 첫 번째 글리프(가장 작은 글리프)를 선택합니다.
   * :menuselection:`요소 --> 글리프 정보 --> 대체 하위`
   * [새로 만들기]를 누릅니다.
   * 풀다운 목록에서 태그를 ``TeX 글리프 목록``으로 설정합니다.
   * 구성 요소 필드에 글리프 목록의 다른 모든 글리프 이름을 입력합니다.

     따라서 charlist가 왼쪽 괄호용인 경우

     lparen 글리프를 선택하고 "lparen.big lparen.bigger lparen.biggest"를 입력할 수 있습니다.

.. _PfaEdit-TeX.extension:

.. object:: 선택적으로 확장 글리프 설정.

   * 글리프가 charlist에 있는 경우 글리프 목록의 마지막 글리프(가장 큰 글리프)를 선택합니다.
   * :menuselection:`요소 --> 글리프 정보 --> 다중 하위`
   * [새로 만들기]를 누릅니다.
   * 풀다운 목록에서 태그를 ``TeX 확장 목록``으로 설정합니다.
   * 이 글리프가 분해되는 글리프의 이름을 위, 중간, 아래, 확장 순서로 입력합니다.

     필드가 없으면 .notdef를 사용합니다.

     따라서 확장 목록이 왼쪽 괄호용인 경우

     lparen.biggest 글리프를 선택하고 "lparen.top lparen.mid lparen.bottom lparen.ext"를 입력할 수 있습니다.

참고: FontForge가 tfm 파일에서 :menuselection:`파일 --> 기능 정보 병합`을 수행하면 이러한 값을 적절하게 설정합니다.

이 작업을 만족스럽게 수행했다면 tfm 파일을 생성할 준비가 된 것입니다. :menuselection:`파일 --> 글꼴 생성`으로 이동하여 풀다운 목록에서 포스트스크립트 인코딩 중 하나를 선택하고 [옵션] 버튼을 누른 다음 [*] Tfm 및 Enc 확인란을 켭니다.

아직 이 파일로 무엇을 *하는지*는 잘 모르겠지만, 이것으로 생성해야 합니다.


.. _PfaEdit-TeX.TeX-Install:

TeX용 type1(pfb) 포스트스크립트 글꼴 설치
------------------------------------------------

저는 TeX/LaTeX 초보 사용자이므로 제 의견은 참고로만 받아들이십시오. 제 시스템에서 이 과정을 성공적으로 수행했습니다.

TeX용 PostScript 글꼴을 설치하는 것은 예상보다 복잡합니다(그리고 지금까지 라틴어 글꼴을 설치하는 방법만 알아냈습니다). 글꼴 파일을 일부 표준 디렉터리로 이동하는 대신 다음을 수행해야 합니다.

* <로컬 추가에 대비하여 TeX 구성>
* TeX가 이해하는 형식으로 파일 이름을 변경합니다.

  (이것이 필요하지 않다고 들었지만, 이것 없이는 작동하지 않았습니다. 아마도 오래된 시스템을 가지고 있거나 충분히 노력하지 않았을 것입니다.)
* TeX가 자체 목적으로 사용할 여러 도우미 파일을 만듭니다.
* 각 파일 유형을 자체 특수 디렉터리로 이동합니다.
* 선택 사항: LaTeX가 글꼴을 쉽게 찾을 수 있도록 패키지 파일을 만듭니다.
* 선택 사항: 패키지 파일을 자체 디렉터리로 이동합니다.
* updmap 스크립트를 사용하거나 수동으로 다음을 수행합니다.

  * dvips의 구성 파일을 업데이트하여 포스트스크립트 글꼴을 찾을 위치를 알 수 있도록 합니다.
  * 선택 사항: pdftex의 구성 파일을 업데이트하여 찾을 위치를 알 수 있도록 합니다.

더 읽기 전에 웹에서 다음 리소스를 살펴보는 것이 좋습니다.

* 표준 TeX 디렉터리 구조에 글꼴을 추가할 수 있지만, TeX 전문가는 TeX 업데이트를 어렵게 만들기 때문에 이를 권장하지 않습니다. 대신 일부 병렬 디렉터리에서 모든 변경을 수행하고 이를 수행하는 방법에 대한 지침을 제공합니다. `TeX 글꼴 및 디렉터리에 대한 설치 조언 <http://www.ctan.org/installationadvice/>`__. 또한 글꼴 설치 예제를 제공하지만 `LaTeX 글꼴 FAQ <http://www.ctan.org/tex-archive/info/Type1fonts/fontinstallationguide.pdf>`__에서 더 잘 설명되어 있습니다.
* 이전 버전의 TeX(예: 제 것)는 여전히 DOS 파일 이름의 8자 제한에 대해 걱정합니다. 이것은 제가 사용하던 도구가 이해할 수 있는 파일 이름을 허용하지 않고 대신 `TeX 글꼴 파일 이름 지정 규칙 <http://www.tug.org/fontname/html/index.html>`__에 설명된 형식을 요구한다는 것을 의미합니다. 자신의 글꼴을 만드는 경우 다음과 같이 요약됩니다.

  * 글꼴의 첫 글자는 "f"여야 합니다(글꼴이 대형 글꼴 공급업체에서 만들지 않았음을 의미함).
  * 다음 두 글자는 글꼴의 패밀리 이름에 대한 약어여야 합니다.
  * 다음 글자(또는 두 글자)는 로만 글꼴의 경우 "r", 이탤릭체의 경우 "i", 사체의 경우 "o", 굵은체의 경우 "b", 굵은 이탤릭체의 경우 "bi"여야 합니다.
  * 마지막 두 글자는 "8a"여야 합니다(글꼴이 Adobe 표준 인코딩임을 의미함). 그리고 글꼴은 *반드시* 해당 인코딩이어야 합니다. 그렇지 않으면 작동하지 않습니다.

    (다시 말하지만, TeX에 등록하는 한 모든 인코딩을 사용할 수 있다고 들었습니다. 이것을 작동시키지 못했습니다. 하지만 제 시스템은 구식입니다.)
* 마지막으로 `LaTeX 글꼴 FAQ <http://www.ctan.org/tex-archive/info/Type1fonts/fontinstallationguide.pdf>`__는 글꼴을 설치하는 방법에 대한 자세한 내용을 설명합니다. 유일한 (사소한) 단점은 Adobe에서 글꼴을 설치한다고 가정한다는 것입니다. 이것은 쉽게 넘어갈 수 있습니다.

  * Adobe의 글꼴은 "f"가 아닌 "p"로 시작하는 글꼴 이름을 가져야 합니다.
  * adobe의 글꼴 패밀리에서 2자 약어로의 변환은 이미 완료되었습니다. adobe 글꼴을 사용할 때 패밀리를 표에서 찾아 2자 약어를 얻고, 자신의 글꼴을 만들 때 자신의 것을 만듭니다.
  * adobe의 공급업체 디렉터리는 "adobe"이지만, 직접 만드는 글꼴의 공급업체 디렉터리는 "public"이어야 합니다.
  * (위의 링크를 읽은 후 이러한 의견이 이해되기를 바랍니다.)
* 트루타입 글꼴로 작업하는 방법을 시도하지 않았지만, 이에 대해 어느 정도 이야기하는 문서가 있습니다. `LaTeX 및 TTF <http://www.radamir.com/tex/ttf-tex.htm>`__
* 기본 사항에 관심이 있다면 `fontinst <http://www.ctan.org/tex-archive/fonts/utilities/fontinst/doc/fontinst.ps>`__ 자체에 대한 설명서가 있습니다.
* 아직 키릴 자모(키릴 자모 T2 인코딩이 6a라고 불리는 것을 제외하고), 그리스어 또는 CJK 글꼴을 처리하는 방법을 모릅니다.
* 글꼴이 설치되면 사용하는 방법에 대한 정보가 있습니다. `LaTeX 및 글꼴 <http://www-h.eng.cam.ac.uk/help/tpl/textprocessing/fonts.html>`__

저는 다음을 수행했습니다.

* `설치 조언 <http://www.ctan.org/installationadvice/>`__ 및 `LaTeX 글꼴 FAQ <http://www.ctan.org/tex-archive/info/Type1fonts/fontinstallationguide.pdf>`__에 설명된 대로 디렉터리 구조를 만들었습니다.
* 글꼴(Cupola라고 부를 것임)을 만들었고, 처음에는 TeX 기본 인코딩으로 인코딩했습니다(필요한 모든 문자가 있는지 확인하기 위해).
* 그런 다음 생성하기 직전에 Adobe 표준 인코딩으로 다시 인코딩했습니다(TeX의 fontinst 루틴이 이를 예상하기 때문).
* 글꼴을 "fcur8a.pfb"로 명명하여 생성했습니다.

  * f -- 소규모 글꼴 공급업체에서 제작, 공개 도메인 등
  * cu -- 패밀리 이름 "Cupola"의 약어
  * r -- 로만 서체
  * 8a -- Adobe 표준 인코딩
* 다음 스크립트를 적용했습니다.

  .. code-block:: bash

     #!/bin/bash
     # 글꼴에 맞게 다음 두 줄을 변경해야 합니다.
     # 그 다음 두 줄도 변경해야 할 수 있습니다.
     BASE=fcu
     PACKAGE=cupola
     VENDOR=public
     LOCALTEXMF=/usr/local/share/texmf

     # 나중에 혼동을 줄 수 있는 오래된 파일 제거
     csh -c "rm fi.tex *.mtx *.pl *.vpl"

     # TeX가 필요한 다양한 유용한 파일을 만들도록 작은 스크립트 생성
     echo "\\input fontinst.sty" >fi.tex
     echo "\\latinfamily{$BASE}{}" >>fi.tex
     echo "\\bye" >>fi.tex

     # 해당 스크립트 실행
     tex fi

     # 하지만 해당 파일 중 일부에 대해 조금 더 처리해야 합니다.
     for file in *.pl ; do
     pltotf $file
     done
     for file in *.vpl ; do
     vptovf $file
     done

     # 더 이상 필요 없는 것 제거
     rm fi.tex *.mtx *.pl *.vpl

     # 다양한 구성 요소에 필요한 디렉터리 생성
     mkdir -p $LOCALTEXMF/fonts/type1/$VENDOR/$PACKAGE \
             $LOCALTEXMF/fonts/afm/$VENDOR/$PACKAGE \
             $LOCALTEXMF/fonts/tfm/$VENDOR/$PACKAGE \
             $LOCALTEXMF/fonts/vf/$VENDOR/$PACKAGE \
             $LOCALTEXMF/tex/latex/$VENDOR/$PACKAGE

     # 모든 것을 예상 디렉터리로 이동
     cp $BASE*.pfb $LOCALTEXMF/fonts/type1/$VENDOR/$PACKAGE
     cp $BASE*.afm $LOCALTEXMF/fonts/afm/$VENDOR/$PACKAGE
     mv $BASE*.tfm $LOCALTEXMF/fonts/tfm/$VENDOR/$PACKAGE
     mv $BASE*.vf $LOCALTEXMF/fonts/vf/$VENDOR/$PACKAGE
     mv *$BASE*.fd $LOCALTEXMF/tex/latex/$VENDOR/$PACKAGE

     # 마지막으로 이 글꼴에 대한 LaTeX 패키지 생성(그리고 올바른 위치에 배치)
     echo "\\ProvidesPackage{$PACKAGE}" > $LOCALTEXMF/tex/latex/$VENDOR/$PACKAGE/$PACKAGE.sty
     echo "\\renewcommand{\\rmdefault}{$BASE}" >> $LOCALTEXMF/tex/latex/$VENDOR/$PACKAGE/$PACKAGE.sty
     echo "\\endinput" >> $LOCALTEXMF/tex/latex/$VENDOR/$PACKAGE/$PACKAGE.sty

     # 하지만 맵 파일 업데이트에는 이 스크립트보다 더 많은 지식이 필요했습니다.
     # 그래서 수동으로 수행하도록 남겨 두었습니다.
     echo "*********************************************************************"
     echo 맵 파일을 직접 만들어야 합니다.
     echo 하나는 $LOCALTEXMF/dvips/config/$BASE.map이라고 해야 하고
     echo " 패밀리의 각 파일에 대한 줄을 포함해야 합니다. 하나는 다음과 같을 수 있습니다."
     echo "${BASE}r8a $PACKAGE-Regular \"TexBase1Encoding ReEncodeFont\" <8r.enc <${BASE}r8a.pfb"
     echo 그런 다음 표준 맵 파일을 정의하는 위치를 찾아 config.ps 파일을 변경하고
     echo "p +$BASE.map"
     echo " 뒤에 추가합니다."
     echo 그런 다음 $LOCALTEXMF/pdftex/config/로 이동합니다.
     echo $LOCALTEXMF/dvips/config/$BASE.map의 복사본(또는 링크)을 만들고
     echo pdftex.cfg를 편집하고
     echo "p +$BASE.map"
     echo 을 적절한 위치에 삽입합니다.
