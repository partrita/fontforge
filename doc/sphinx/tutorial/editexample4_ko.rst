강조된 글리프, 합자, 조회 및 기능
==================================


강조된 글리프 빌드
------------------------

라틴어, 그리스어 및 키릴 자모에는 강조된 글리프가 많이 있습니다. FontForge는 기본 글리프에서 강조된 글리프를 빌드하는 여러 가지 방법을 제공합니다.

가장 확실한 메커니즘은 간단한 복사 및 붙여넣기입니다. 문자 "A"를 :ref:`복사 <editmenu.Copy>`하고 "Ã"에 :ref:`붙여넣기 <editmenu.Paste>`한 다음 물결표 악센트를 :ref:`복사 <editmenu.Copy>`하고 "Ã"에 :ref:`붙여넣기 <editmenu.PasteInto>`합니다(붙여넣기는 붙여넣기 전에 글리프를 지우는 반면 붙여넣기는 글리프에 있던 것과 클립보드에 있는 것을 병합한다는 점에서 미묘하게 다릅니다). 그런 다음 "Ã"를 열고 악센트가 A 위에 올바르게 중앙에 오도록 위치를 조정합니다.

이 메커니즘은 특히 효율적이지 않습니다. 문자 "A"의 모양을 변경하면 "A"에서 빌드된 모든 강조된 글리프를 다시 생성해야 합니다. FontForge에는 글리프에 대한 :ref:`참조 <overview.References>` 개념이 있습니다. 따라서 "A"에 대한 참조를 복사하여 붙여넣고 물결표에 대한 참조를 복사하여 붙여넣은 다음 다시 A 위에 악센트 위치를 조정할 수 있습니다.

그런 다음 A의 모양을 변경하면 "Ã"의 A 모양이 자동으로 업데이트되고 "Ã"의 너비도 업데이트됩니다.

하지만 FontForge는 "Ã"가 "A"와 물결표 악센트로 구성되어 있다는 것을 알고 있으며, "Ã"에 참조를 넣고 A 위에 악센트를 배치하여 강조된 글리프를 쉽게 만들 수 있습니다. (유니코드 컨소시엄은 유니코드의 모든 강조된 글리프의 구성 요소를 나열하는 데이터베이스를 제공하며 FontForge는 이를 사용합니다.)

예를 들어, tutorial/Ambrosia.sfd 파일을 열고 인코딩 0xc0-0xff의 모든 글리프를 선택한 다음 :ref:`요소->빌드->강조된 빌드 <elementmenu.Accented>`를 누르면 모든 강조된 글리프가 마법처럼 나타납니다(이 범위에는 강조되지 않은 몇 가지 글리프가 있으며 비어 있습니다).

FontForge에는 악센트 위치를 지정하는 휴리스틱이 있습니다(대부분의 악센트는 글리프의 가장 높은 지점 위에 중앙에 배치됨). 때로는 이로 인해 나쁜 결과가 발생할 수 있습니다( "u"의 두 줄기 중 하나가 다른 것보다 약간 더 크면 악센트가 글리프 위에 중앙에 배치되는 대신 그 위에 배치됨). 따라서 강조된 글리프를 만든 후에는 확인해야 합니다. 하나 또는 두 개를 조정해야 할 수도 있습니다(또는 기본 글리프를 약간 재설계하고 싶을 수도 있습니다).


.. _editexample4.ligature:

합자 만들기
-------------------

유니코드에는 여러 합자 글리프가 포함되어 있습니다(라틴어에는 Æ, OE, fi 등이 있고 아랍어에는 수백 개가 있음). 다시 말하지만 유니코드는 각 표준 합자의 구성 요소를 나열하는 데이터베이스를 제공합니다.

FontForge는 멋진 합자를 만들 수 없지만 할 수 있는 것은 :ref:`요소->빌드->합성 빌드 <elementmenu.Accented>`를 사용하여 합자의 모든 구성 요소를 글리프에 넣는 것입니다. 이렇게 하면 (적어도 라틴어에서는) 합자를 디자인하기가 약간 더 쉬워집니다. 이것이 작동하려면 원본 글리프와 결합 글리프(원과 글리프로 식별됨)가 개요에 있어야 합니다. 둘 중 하나라도 없으면 메뉴 항목이 비활성화됩니다. 글리프가 어떤 구성 요소로 구성되어 있는지 알아보려면 글리프 정보를 검토한 다음 구성 요소 섹션을 검토하십시오.

.. rubric:: 합자 빌드 단계

.. flex-grid::

   * - .. image:: /images/ffi-refs.png
          :alt: 참조로서의 ffi 합자

       :ref:`요소 -> 글리프 정보 <elementmenu.CharInfo>` 대화 상자를 사용하여 글리프 이름을 지정하고 합자로 표시합니다. 그런 다음 :ref:`요소 -> 빌드 -> 합성 빌드 <elementmenu.Accented>`를 사용하여 합자 구성 요소에 대한 참조를 삽입합니다.
     - .. image:: /images/ffi-unlink.png
          :alt: 참조 연결 해제 후 ffi

       :ref:`편집-> 참조 연결 해제 <editmenu.Unlink>` 명령을 사용하여 참조를 윤곽선 세트로 변환합니다.
     - .. image:: /images/ffi-moved.png
          :alt: 첫 번째 f를 낮춘 후 ffi

       구성 요소를 더 잘 보이도록 조정합니다. 여기서 첫 번째 f의 줄기가 낮아졌습니다.
     - .. image:: /images/ffi-rmoverlap.png
          :alt: 겹침 제거 후 ffi

       :ref:`요소 -> 겹침 제거 <elementmenu.Remove>` 명령을 사용하여 글리프를 정리합니다.
     - .. image:: /images/ffi-caret.png
          :alt: 합자 캐럿 조정 후 ffi

       마지막으로 합자 캐럿 선을 원점에서 구성 요소 사이의 더 적절한 위치로 끕니다.

일부 워드 프로세서는 편집 캐럿을 합자 내에 배치할 수 있습니다(합자의 각 구성 요소 사이에 캐럿 위치가 있음). 이것은 해당 워드 프로세서 사용자가 합자를 다루고 있다는 것을 알 필요가 없으며 구성 요소가 있을 때 볼 수 있는 것과 매우 유사한 동작을 본다는 것을 의미합니다. 그러나 워드 프로세서가 이 작업을 수행하려면 적절한 캐럿 위치 위치를 제공하는 폰트 디자이너의 일부 정보가 있어야 합니다. FontForge가 글리프가 합자임을 알아차리자마자 합자의 구성 요소 사이에 맞도록 충분한 캐럿 위치 선을 삽입합니다. FontForge는 이것들을 원점에 배치하며, 원점에 그대로 두면 FontForge는 무시합니다. 하지만 합자를 만든 후에는 포인터 도구를 원점 선으로 이동하고 버튼을 누른 다음 캐럿 선 중 하나를 올바른 위치로 끌 수 있습니다. (Apple Advanced Typography 및 OpenType만 이를 지원합니다.)

인도어 스크립트에는 많은 합자가 필요하지만 유니코드는 이에 대한 인코딩을 제공하지 않습니다. 유니코드의 일부가 아닌 합자를 만들고 싶다면 그렇게 할 수 있습니다. 먼저 :ref:`폰트에 인코딩되지 않은 글리프를 추가 <faq.new-name>`하고(또는 폰트가 유니코드 폰트인 경우 개인 사용 영역의 코드 포인트를 사용할 수 있음) 글리프 이름을 지정합니다. 이름이 중요합니다. 올바르게 이름을 지정하면 FontForge가 합자임을 알아내고 구성 요소가 무엇인지 알아낼 수 있습니다. "longs", "longs" 및 "l" 글리프로 합자를 만들려면 "longs_longs_l"로 이름을 지정하고, 유니코드 0D15, 0D4D 및 0D15로 합자를 만들려면 "uni0D15_uni0D4D_uni0D15"로 이름을 지정합니다.

합자 이름을 지정하고 구성 요소를 삽입한 후(합성 빌드 사용)에는 아마도 글리프를 열고 :ref:`참조 연결을 해제 <editmenu.Unlink>`하고 편집하여 만족스러운 모양을 만들고 싶을 것입니다(위와 같이).


.. _editexample4.lookups:

조회 및 기능
--------------------

.. image:: /images/fontinfo-lookups.png
   :align: right

불행히도 합자 글리프를 만드는 것만으로는 충분하지 않습니다. 글리프가 합자라는 정보와 어떤 구성 요소로 구성되었는지에 대한 정보를 폰트에 포함해야 합니다.

OpenType에서는 이것이 조회 및 기능으로 처리됩니다. 조회는 변환 정보가 포함된 폰트의 테이블 모음입니다. 기능은 조회 모음이며 폰트 외부 세계에 해당 조회 세트가 무엇을 할 것으로 예상되는지에 대한 의미 정보를 제공합니다. 따라서 위 예에서 조회에는 "f" + "f" + "i"가 "ffi"로 바뀌어야 한다는 정보가 포함되고, 기능에는 이것이 라틴어 스크립트의 표준 합자라는 정보가 포함됩니다.

따라서 처음으로 합자 글리프를 만들 때 해당 글리프에 대한 정보가 저장될 조회(및 조회 하위 테이블)를 만들어야 합니다. 후속 합자는 아마도 동일한 조회 및 하위 테이블을 공유할 수 있습니다.

(이것은 라틴어 합자에는 과도해 보일 수 있으며 아마도 그럴 것입니다. 하지만 복잡성은 더 복잡한 쓰기 시스템에 필요합니다.)

:ref:`요소->폰트 정보 <fontinfo.Lookups>` 명령의 조회 창을 열고 ``[조회 추가]`` 버튼을 누릅니다. 그러면 새 조회 속성을 채울 수 있는 새 대화 상자가 나타납니다.

.. image:: /images/AddLookup-Liga.png
   :align: left

먼저 조회 유형을 선택해야 합니다. 합자의 경우 "합자 대체"여야 합니다. 그런 다음 이 조회를 기능, 스크립트 및 언어 세트에 바인딩할 수 있습니다. "ffi" 합자는 라틴어 조판에서 표준 합자이므로 'liga' 태그와 'latn' 스크립트에 바인딩해야 합니다. ("liga" 오른쪽의 작은 상자를 클릭하면 기능의 소위 "친숙한 이름"의 풀다운 목록이 나타납니다. "liga"는 "표준 합자"에 해당합니다.)

언어는 약간 까다롭습니다. 이 합자는 라틴어 스크립트를 사용하는 터키어를 제외한 모든 언어에 대해 활성화되어야 합니다(터키어는 점 없는 i를 사용하며 "fi" 및 "ffi" 합자의 "i"에 점이 있는지 여부는 명확하지 않음). 따라서 터키어를 제외한 모든 언어를 나열하고 싶습니다. 그것은 많은 언어입니다. 대신 규칙은 언어가 폰트의 어느 곳에도 명시적으로 언급되지 않은 경우 해당 언어는 "기본" 언어로 처리된다는 것입니다. 따라서 이 기능을 터키어에 대해 활성화하지 않으려면 언어 목록에 터키어를 구체적으로 언급하는 다른 기능을 만들어야 합니다.

기능 목록 아래에는 플래그 세트가 있습니다. 라틴어 합자에서는 이러한 플래그 중 어느 것도 설정할 필요가 없습니다. 아랍어에서는 "오른쪽에서 왼쪽으로"와 "결합 표시 무시"를 모두 설정하고 싶을 수 있습니다.

다음으로 조회에 이름을 지정해야 합니다. 이 이름은 사용자를 위한 것이며 실제 폰트에서는 절대 볼 수 없습니다. 그러나 이름은 다른 모든 조회의 이름과 구별되어야 합니다.

마지막으로 이 조회의 합자를 afm 파일에 저장할지 여부를 결정합니다.

.. image:: /images/subtable-ffi.png
   :align: right

조회를 만든 후에는 해당 조회에 하위 테이블을 만들어야 합니다. 조회 줄(폰트 정보의 조회 창에서)을 선택하고 ``[하위 테이블 추가]``를 누릅니다. 이것은 매우 간단한 대화 상자입니다... 단순히 하위 테이블의 이름을 제공하면 다른 대화 상자가 나타나고 (마침내) 합자 정보를 저장할 수 있습니다.

.. warning::

   OpenType 엔진은 현재 스크립트에 적합하다고 생각하는 기능만 적용합니다(라틴어 스크립트에서 Uniscribe는 'liga'를 적용함). 더 나쁜 것은 일부 응용 프로그램은 어떤 기능도 적용하지 않도록 선택할 수 있다는 것입니다(Word는 라틴어에서 합자를 수행하지 않음 -- 2007년 릴리스에서 변경되었을 수 있음?). `Microsoft는 Uniscribe에서 어떤 스크립트에 어떤 기능을 적용하는지 문서화하려고 시도 <http://www.microsoft.com/typography/specs/default.htm>`__하지만 Word와 Office는 기본값과 상당히 다른 동작을 하므로 그다지 도움이 되지 않습니다.
