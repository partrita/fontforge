스피로를 사용하여 글리프 만들기
=============================

스피로는 `Raph Levien <https://levien.com/spiro/>`_의 작품으로, 베지에 스플라인에 사용되는 전통적인 곡선 위 및 곡선 밖 점의 혼합 대신 곡선 위 점 세트를 사용하여 글리프를 디자인하는 대체 방법을 제공합니다.

-  G4 곡선 점 (4차 도함수까지 연속)
-  G2 곡선 점 (2차 도함수까지 연속)
   기본적으로 이것은 다음과 같이 요약됩니다. 날카로운 곡선이 있는 경우 G2 점을 사용하는 것이 좋고, 더 완만한 곡선은 G4 점을 사용하는 것이 좋습니다.
-  모서리 점
-  이전 제약 점 (윤곽선이 곡선에서 직선으로 바뀔 때 사용) |spiroprevconstraint|
-  다음 제약 점 (윤곽선이 직선에서 곡선으로 바뀔 때 사용) |spironextconstraint|

.. |spiroprevconstraint| image:: /images/spiroprevconstraint.png
.. |spironextconstraint| image:: /images/spironextconstraint.png


1단계
******

이전과 같이 Ambrosia의 "C" 글리프를 편집해 보겠습니다.

.. figure:: /images/Cspiro0.png

다시 빈 글리프로 시작합니다. 도구 창에 나선 모양의 버튼이 있습니다. 이 버튼을 누르면 스피로 모드로 들어가고 사용할 수 있는 도구가 약간 변경됩니다.

(버튼을 다시 누르면 베지에 모드로 돌아갑니다)

다시 파일->가져오기를 사용하여 배경 이미지를 가져온 다음 적절하게 크기를 조정합니다. (이 작업을 수행하는 방법을 모르는 경우 :ref:`이전 페이지 <editexample.Import>`를 참조하십시오)


2단계
******

.. figure:: /images/Cspiro1.png

G4 곡선 점(세 번째 행의 왼쪽에 있는 도구)을 선택합니다.

G4 곡선 점은 양쪽의 스플라인 기울기가 동일하고 스플라인의 곡률도 동일하다는 좋은 속성을 가지고 있습니다.

그런 다음 포인터를 이미지 위로 이동하고 클릭하여 비트맵 이미지 가장자리의 한 점에 점을 배치합니다.

3단계
******

.. figure:: /images/Cspiro2.png

곡선에서 직선으로 윤곽선을 멋지게 변경하는 "다음 제약" 점을 선택합니다.

잘못된 제약 조건을 선택하면(저는 종종 그렇게 합니다 -- 나중에 윤곽선이 여기서 왜곡되어 보일 때 분명해집니다), 제약 조건을 선택하고 :ref:`요소->정보 가져오기 <getinfo.Spiro>`를 사용하여 점 유형을 변경하거나 :doc:`점 메뉴 </ui/menus/pointmenu>`를 사용합니다.

4단계
******

.. figure:: /images/Cspiro3.png

이제 도구 메뉴에서 모서리 점(사각형처럼 생긴 것)을 선택합니다.

기울기가 급격하게 변하는 위치, 즉 모서리에 배치합니다.

이제 "왼쪽 접선 점"에 대해 이야기할 준비가 되었습니다. 모서리 점에서 접선 점을 향해 서 있다고 상상해 보십시오. 그 다음 점(이 경우 곡선 점)이 왼쪽에 있습니까, 아니면 오른쪽에 있습니까? 왼쪽에 있으면 왼쪽 접선을 사용하고, 오른쪽에 있으면 오른쪽 접선을 사용합니다.

5단계
******

.. figure:: /images/Cspiro4.png

이제 "C"의 상단에서 약간 까다로운 작업을 하고 싶습니다. 여기에는 두 모서리 사이에 약간의 곡선이 있는 세리프가 있고, 두 개의 급격한 방향 변경이 있습니다.

더 정밀하게 작업하려면 이미지의 확대 보기가 필요하므로 도구 창에서 돋보기 도구를 선택하고 세리프 중앙으로 이동한 다음 세리프가 화면을 채울 때까지 여러 번 클릭합니다.

6단계
******

.. figure:: /images/Cspiro5.png

일반적으로 모서리 점은 양쪽에 제약 조건(또는 다른 모서리) 점이 있어야 하므로 다른 제약 조건을 선택해야 합니다. 이 경우 윤곽선이 직선에서 곡선으로 변경되므로 "이전 제약 조건" 점을 의미합니다.

7단계
******

.. figure:: /images/Cspiro6.png

그런 다음 세리프의 부드러운 곡선을 만드는 데 필요한 나머지 점을 채웁니다.

8단계
******

.. figure:: /images/Cspiro6_5.png

그리고 세리프의 다른 쪽의 또 다른 부드러운 곡선.

9단계
******

.. figure:: /images/Cspiro7.png

이제 이미지의 확대 보기가 더 이상 유용하지 않으므로 돋보기 도구를 다시 잡고 Alt(Meta, Option) 키를 누릅니다. 커서가 변경되고 클릭하면 축소됩니다.

그런 다음 이쪽의 나머지 점을 채웁니다.

10단계
*******

.. figure:: /images/Cspiro8.png

C의 아래쪽 끝에 가까워지면 다시 확대해야 합니다.

11단계
*******

.. figure:: /images/Cspiro9.png

그리고 결국 글리프의 대략적인 윤곽을 완성했습니다. 시작점을 클릭하면 곡선이 닫힙니다.

불행히도 결과는 우리가 기대했던 것과 약간 다릅니다. 약간 불규칙한 돌출부가 있습니다.

다음과 같이 수정할 수 있습니다.

    #. 점 이동

       포인터 도구를 사용하고 점을 클릭하거나(또는 시프트 키를 누른 채 여러 점을 선택) 주위를 드래그합니다.
    #. 윤곽선에 새 점 추가.

       적절한 스피로 도구를 사용하여 윤곽선 어딘가에서 마우스를 누릅니다. 새 점이 나타납니다. 이제 이 점을 주위로 드래그할 수 있습니다.

.. figure:: /images/Cspirals.png

수정 과정에서 점을 너무 멀리 이동하여 스피로 변환기가 이해할 수 없게 될 수 있습니다. 갑자기 (거의) 멋진 윤곽선이 불규칙한 나선으로 변합니다.

걱정하지 마십시오. 점을 다시 이동하면 정상으로 돌아갑니다. 점을 너무 멀리 이동하면 상황이 더 나빠져 윤곽선이 완전히 사라질 수 있습니다. 그것에 대해서도 걱정하지 마십시오. 그냥 점을 다시 제자리에 놓으십시오. 또는 편집->실행 취소를 사용하십시오.

그리고 의도치 않게 만든 나선의 기묘한 아름다움을 즐기십시오.

(Raph가 이 작업을 하고 있으며 어느 시점에는 나선이 완전히 사라질 수도 있지만, 약간의 매력이 있습니다. 사라지는 것을 보면 아쉬울 것입니다.)

.. figure:: /images/Cspiro10.png
