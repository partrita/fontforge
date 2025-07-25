FontForge 스크립팅 튜토리얼
============================

.. note::

  FontForge는 이제 파이썬 스크립팅을 제공합니다. 파이썬에 익숙하다면 아마도 더 나은 선택일 것입니다. `파이썬 <http://www.python.org/doc/>`__에 대한 많은 정보가 있으며, 반복하지 않겠습니다. FontForge의 파이썬 추가 사항은 :doc:`여기 </scripting/python>`에 문서화되어 있습니다.

저는 상당히 기초적인 수준을 유지하려고 노력하지만, 이것은 프로그래밍을 가르치려는 시도가 *아닙니다*.


간단한 예제
----------------

Type1 PostScript 글꼴(pfb/afm 조합)이 있고 이를 TrueType 글꼴로 변환하고 싶다고 가정해 봅시다. 이를 수행할 수 있는 스크립트는 어떤 모습일까요?

UI로 이 작업을 수행하는 경우 먼저 :ref:`파일->열기 <filemenu.Open>`로 글꼴을 연 다음 :ref:`파일->생성 <filemenu.Generate>`으로 트루타입 글꼴을 생성합니다. 스크립트를 작성할 때도 본질적으로 동일한 작업을 수행합니다.


초기 솔루션
^^^^^^^^^^^^^^^^

::

   Open($1)
   Generate($1:r + ".ttf")

일반적으로 메뉴 명령과 동일한 이름의 스크립팅 함수가 있습니다(음, 메뉴 명령의 영어 이름과 동일).

':ref:`$1 <scripting.variables>`'은 마법입니다. 이것은 :ref:`스크립트에 전달된 첫 번째 인수 <scripting-tutorial.Invoking>`를 의미합니다.

':ref:`$1:r + ".ttf" <scripting.Expressions>` '은 훨씬 더 복잡한 마법입니다. 이것은 '첫 번째 인수($1)를 가져와 확장자(아마도 ".pfb"일 것임)를 제거한 다음 파일 이름에 ".ttf" 문자열을 추가'하는 것을 의미합니다.

Generate 스크립팅 명령은 지정한 파일 이름의 확장자에 따라 생성할 글꼴 유형을 결정합니다. 여기서는 "ttf" 확장자를 지정하여 트루타입을 의미합니다.

afm 파일을 로드하려고 시도하지 않는다는 점에 유의하십시오. Open 명령이 pfb와 동일한 디렉토리에 있는 경우 자동으로 이 작업을 수행하기 때문입니다.


실제 고려 사항
^^^^^^^^^^^^^^^^^^^^^^^^^

이제 스크립트가 어떻게 생겼는지 알았습니다. 유용하게 사용하려면 자체 파일에 있어야 합니다. 따라서 "convert.pe"라는 파일을 만들고 위의 스크립트를 저장하십시오.

하지만 더 유용하게 사용하려면 스크립트 시작 부분에 주석 줄을 추가해야 합니다(주석 줄은 '#' 문자로 시작하는 줄입니다).

::

   #!/usr/local/bin/fontforge
   Open($1)
   Generate($1:r + ".ttf")

그렇게 한 후 다음을 입력하십시오.

::

   $ chmod +x convert.pe

이 주석은 fontforge에는 중요하지 않지만 다음 섹션에서 볼 수 있듯이 유닉스 셸에는 의미가 있습니다.


.. _scripting-tutorial.Invoking:

스크립트 호출 및 인수 전달
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

자, 이제 기본 스크립트가 있습니다. 어떻게 사용하나요?

음, 다음과 같이 입력하여 FontForge에 직접 전달할 수 있습니다.

::

   $ fontforge -script convert.pe foo.pfb

하지만 위의 주석을 추가했다면 다음과 같이 입력할 수도 있습니다.

::

   $ convert.pe foo.pfb

그리고 셸은 스크립트를 처리하기 위해 fontforge를 호출해야 한다는 것을 알고 있습니다.


루프 사용
^^^^^^^^^^^

그것은 모두 좋지만 변환할 글꼴이 많으면 지루할 수 있습니다. 따라서 한 번에 하나씩 처리할 수 있도록 스크립트를 변경하여 많은 파일 이름을 사용할 수 있도록 합시다.

::

   #!/usr/local/bin/fontforge
   i=1
   while ( i<$argc )
     Open($argv[i])
     Generate($argv[i]:r + ".ttf")
     i = i+1
   endloop

여기서 우리는 ``$argc``와 ``$argv``라는 변수를 도입했습니다. 첫 번째는 이 스크립트에 전달된 인수의 수이고, 두 번째는 모든 인수를 포함하는 배열이며, ``$argv[i]``는 전달된 i번째 인수를 의미합니다.

그런 다음 다음이 있습니다.

::

   i=1

이것은 "i"라는 로컬 변수가 있고 값 1을 할당함을 선언합니다.

while 루프는 ``( i<$argv )`` 조건이 참인 동안 "``while``" 키워드와 "``endloop``" 키워드 사이의 모든 문을 실행합니다. 즉, 변환할 인수가 더 있는 한 루프는 계속됩니다.

그리고 다음과 같이 이 스크립트를 호출할 수 있습니다.

::

   $ convert.pe *.pfb

또는 비슷한 것.


복잡성
^^^^^^^^^^^^

이제 트루타입 글꼴을 오픈타입 글꼴로 변환할 수 있는 스크립트와 타입1 글꼴을 트루타입으로 변환할 수 있는 스크립트가 필요하다고 가정해 봅시다. 스크립트를 더 복잡하게 만들어 보겠습니다.

::

   #!/usr/local/bin/fontforge
   i=1
   format=".ttf"
   while ( i<$argc )
     if ( $argv[i]=="-format" || $argv[i]=="--format" )
       i=i+1
       format = $argv[i]
     else
       Open($argv[i])
       Generate($argv[i]:r + format)
     endif
     i = i+1
   endloop

그리고 다음과 같이 호출할 수 있습니다.

::

   $ convert.pe --format ".ttf" *.pfb --format ".otf" *.ttf

이제 앞으로 사용할 출력 유형을 포함하는 ``format``이라는 새 변수가 있습니다. 이를 트루타입 ".ttf"로 초기화하지만 사용자가 "--format"(또는 "-format")이라는 인수를 제공하면 출력을 사용자가 요청한 것으로 변경합니다.

여기서 "``if``" 문을 도입했습니다. 이 문은 ``( $argv[i]=="-format" || $argv[i]=="--format" )`` 조건이 참이면 "``if``"와 "``else``" 사이의 문을 실행하고, 그렇지 않으면 "``else``"와 "``endif``" 사이의 문을 실행합니다. || 연산자는 "또는"을 의미하므로 $argv[i]가 "-format" 또는 "--format"이면 조건이 참입니다.

다음 사항을 확인하기 위해 오류 검사를 수행해야 합니다.

* ``format`` 변수에 저장할 다른 인수가 있었는지 확인합니다.
* 인수에 합리적인 값이 포함되었는지 확인합니다(.ttf, .pfb, .otf, .svg, ...).

::

   #!/usr/local/bin/fontforge
   i=1
   format=".ttf"
   while ( i<$argc )
     if ( $argv[i]=="-format" || $argv[i]=="--format" )
       i=i+1
       if ( i<$argc )
         format = $argv[i]
         if ( format!=".ttf" && format!=".otf" && \
             format!=".pfb" && format!=".svg" )
           Error( "Expected one of '.ttf', '.otf', '.pfb' or '.svg' for format" )
         endif
       endif
     else
       Open($argv[i])
       Generate($argv[i]:r + format)
     endif
     i = i+1
   endloop

긴 줄이 있을 때 백슬래시를 사용하여 두 줄로 나눈 것을 확인하십시오. 일반적으로 줄 끝은 문 끝을 표시하므로 문이 다음 줄로 계속됨을 나타내려면 백슬래시가 필요합니다.

이제 원한다면 트루타입 글꼴에서 유효한 포스트스크립트 글꼴을 생성할 것입니다... 하지만 그 변환을 개선할 수 있습니다.

::

   #!/usr/local/bin/fontforge
   i=1
   format=".ttf"
   while ( i<$argc )
     if ( $argv[i]=="-format" || $argv[i]=="--format" )
       i=i+1
       if ( i<$argc )
         format = $argv[i]
         if ( format!=".ttf" && format!=".otf" && \
             format!=".pfb" && format!=".svg" )
           Error( "Expected one of '.ttf', '.otf', '.pfb' or '.svg' for format" )
         endif
       endif
     else
       Open($argv[i])
       if ( $order==2 && (format==".otf" || format==".pfb" ))
         SetFontOrder(3)
         SelectAll()
         Simplify(128+32+8,1.5)
         ScaleToEm(1000)
       elseif ( $order==3 && format==".ttf" )
         ScaleToEm(2048)
         RoundToInt()
       endif
       Generate($argv[i]:r + format)
     endif
     i = i+1
   endloop

그 특성상 트루타입 글꼴은 포스트스크립트 글꼴보다 더 많은 점을 포함하지만, 한 형식에서 다른 형식으로 변환할 때 Simplify 명령을 사용하여 그 수를 줄일 수 있습니다. 또한 PostScript 글꼴은 em당 1000 단위여야 하고 TrueType 글꼴은 em당 2의 거듭제곱 단위(일반적으로 2048 또는 1024)여야 하므로 이러한 규칙을 적용합니다. 마지막으로 TrueType 글꼴은 점에 대해 정수(또는 경우에 따라 반정수) 좌표만 지원합니다.


다른 예제
--------------


강조된 문자 추가
^^^^^^^^^^^^^^^^^^^^^^^^^^

전체 유니코드 범위의 강조된 문자를 가진 Type1 글꼴은 거의 없습니다. FontForge를 사용하면 Type1 글꼴을 로드하고 글꼴이 허용하는 한 가능한 많은 강조된 문자를 추가하는 것이 매우 쉽습니다(글꼴에 오고넥이 포함되어 있지 않으면 FF는 A오고넥을 만들 수 없음).

::

   #!/usr/local/bin/fontforge
   Open($1)
   Reencode("unicode")
   SelectWorthOutputting()
   SelectInvert()
   BuildAccented()
   Generate($1:r + ".otf")


type1 및 type1 전문가 글꼴 병합 및 적절한 GSUB 테이블 생성.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Adobe는 일반 글꼴과 소문자, 소문자 숫자 등을 포함하는 "전문가" 글꼴이 포함된 글꼴 팩을 배송했습니다. 이제는 글리프를 연결하는 적절한 GSUB 항목이 있는 하나의 otf 글꼴에 모두 채워져야 합니다.

::

   #!/usr/local/bin/fontforge
   Open($1)
   MergeFonts($2)
   RenameGlyphs("AGL with PUA")
   SelectAll()
   DefaultATT("*")


더 많은 예제
^^^^^^^^^^^^^

:ref:`스크립팅 페이지 <scripting.Example>`를 참조하십시오.
