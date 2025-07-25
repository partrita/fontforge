데이터 유형
==========

.. highlight:: c

.. warning::
   설명된 내용 중 많은 부분이 여전히 관련이 있지만, 최근 코드 변경 사항으로 업데이트되지 않았습니다. `리포지토리 <https://github.com/fontforge/fontforge>`__의 현재 상태와 교차 참조하십시오.

이것은 두 가지 다른 유형의 글꼴, 즉 SplineFont(포스트스크립트 또는 트루타입 글꼴)와 BDFFont(비트맵 글꼴, 본질적으로 bdf 파일)를 설명합니다. 두 데이터 구조 사이에는 몇 가지 유사점이 있으며, 각 데이터 구조는 기본적으로 문자의 배열(구멍이 있을 수 있음)입니다. 각 글꼴에는 글꼴 이름과 같은 정보도 포함되어 있습니다.

:ref:`SplineFont <splinefont.SplineFont>`는 여러 개의 :ref:`SplineChars <splinefont.SplineChar>`로 구성되며, 각 SplineChar에는 네 개의 레이어가 포함됩니다.

* 최종 글꼴에 포함될 모든 흥미로운 내용이 포함된 전경 레이어

  * 일련의 경로(:ref:`SplineSet <splinefont.SplinePointList>` 또는 :ref:`SplinePointList <splinefont.SplinePointList>`라고 함, 죄송합니다. 데이터 구조에 두 가지 다른 이름을 부여했습니다.)
  * 다른 문자에 대한 일련의 참조(:ref:`RefChar <splinefont.RefChar>`라고 함)
  * 너비
  * :ref:`실행 취소 <splinefont.Undoes>`
* 문자 이미지를 추적할 수 있는 배경 레이어이며 다음을 포함할 수 있습니다.

  * 일련의 :ref:`SplineSet <splinefont.SplinePointList>`
  * 일련의 :ref:`이미지 <splinefont.ImageList>`
  * :ref:`실행 취소 <splinefont.Undoes>`
* 글꼴의 모든 문자가 공유하고 Cap Height, X Height 등을 나타내는 선을 넣을 수 있는 그리드 레이어. 실제로는 무엇이든 넣을 수 있지만 이것이 분명히 유용한 것입니다. 모든 문자에서 볼 수 있습니다. 다음을 포함합니다.

  * 일련의 :ref:`SplineSet <splinefont.SplinePointList>`
  * :ref:`실행 취소 <splinefont.Undoes>`
* 힌트 레이어는 다른 어떤 것과도 완전히 다른 형식이며 수평 및 수직 힌트로 구성됩니다. 힌트는 시작 위치와 너비(줄기를 나타냄. 너비는 음수일 수 있음)만으로 구성됩니다.

  * 일련의 :ref:`힌트 <splinefont.Hints>`. 힌트는 편집을 위해 힌트 메뉴의 특수 도구가 필요합니다.
  * **!!!! 힌트 작업을 실행 취소할 방법이 없습니다!!!!**

다중 레이어 모드에서는 획을 긋거나 채울 수 있는 많은 전경 레이어가 있을 수 있습니다. 비트맵 이미지도 포함될 수 있습니다.

모든 SplineChar에는 이름, 유니코드 인코딩 및 글꼴의 원래 위치가 연결되어 있습니다.

SplineSet은 시작점과 끝점, 그리고 그것들을 연결하는 모든 스플라인으로 구성됩니다. 시작점과 끝점은 동일할 수 있으며, 이는 닫힌 경로 또는 한 점만 있는 퇴화된 경로를 나타냅니다.

SplinePoint에는 점의 x, y 위치와 해당 점에 연결된 두 제어점의 위치가 포함됩니다. 제어점은 주 점과 일치할 수 있습니다. SplinePoint는 두 개의 스플라인, 즉 이전 스플라인과 다음 스플라인에 연결할 수 있습니다. 스플라인에는 두 개의 SplinePoint(시작 및 끝점)에 대한 포인터가 포함되어 있으며, 이 두 점이 설명하는 베지어 곡선의 매개변수(x = a*t^3+b*t^2+c*t+d, y=ditto)도 포함됩니다. 두 개의 의미 있는 제어점이 각각의 SplinePoint와 일치하면 스플라인은 직선입니다. 스플라인에는 order2(트루타입 2차 형식) 또는 order3(포스트스크립트 3차 형식)인지 여부를 나타내는 비트가 포함됩니다. 2차인 경우 실제로는 제어점이 하나만 있으며, ff는 시작 및 끝 SplinePoint의 제어점이 동일한 위치를 갖는 규칙을 사용합니다.

글꼴의 인코딩은 EncMap이라는 별도의 데이터 구조에 저장됩니다. 이 구조는 어떤 글리프가 어디로 가는지 제어하고 인코딩 이름을 제공하는 인코딩 구조에 대한 포인터도 포함합니다. EncMap에는 ``map``과 ``backmap``이라는 두 개의 배열이 포함됩니다. 맵은 문자의 인코딩으로 인덱싱되고 관련 글리프의 위치(관련 스플라인 글꼴의 글리프 목록에서)를 제공합니다. 백맵은 역 매핑을 제공하며, 스플라인 글꼴의 글리프 위치로 인덱싱되고 해당 글리프에 대한 인코딩을 제공합니다. (참고: 맵 배열은 여러 인코딩을 하나의 글리프에 매핑할 수 있으며, 백맵은 그 중 하나만 나타냅니다.)

:ref:`BDFFont <splinefont.BDFFont>`는 항상 (비어 있을 수 있는) SplineFont 및 해당 EncMap과 연결되어 있으며, 각 :ref:`BDFChars <splinefont.BDFChar>` 배열로 구성되며 각 배열에는 다음이 포함됩니다.

* 비트맵
* 부동 선택 사항일 수 있음
* 이름
* :ref:`실행 취소 <splinefont.Undoes>`
* 연결된 :ref:`SplineChar <splinefont.SplineChar>`에 대한 포인터

배열은 SplineFont의 배열과 동일한 방식으로 정렬되며 동일한 EncMap을 둘 다에 사용할 수 있습니다.

::

   /* Copyright (C) 2000-2003 by George Williams */
   /*
    * Redistribution and use in source and binary forms, with or without
    * modification, are permitted provided that the following conditions are met:

    * Redistributions of source code must retain the above copyright notice, this
    * list of conditions and the following disclaimer.

    * Redistributions in binary form must reproduce the above copyright notice,
    * this list of conditions and the following disclaimer in the documentation
    * and/or other materials provided with the distribution.

    * The name of the author may not be used to endorse or promote products
    * derived from this software without specific prior written permission.

    * THIS SOFTWARE IS PROVIDED BY THE AUTHOR ``AS IS'' AND ANY EXPRESS OR IMPLIED
    * WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
    * MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
    * EVENT SHALL THE AUTHOR BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
    * SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
    * PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
    * OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
    * WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
    * OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
    * ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
    */
   #ifndef _SPLINEFONT_H
   #define _SPLINEFONT_H

   #include "basics.h"
   #include "charset.h"

   enum linejoin {
       lj_miter,           /* 선이 만날 때까지 연장 */
       lj_round,           /* 확장 반경의 접합부에 중심을 둔 원 */
       lj_bevel            /* 다음 선과 이전 선의 끝 사이의 직선 */
   };
   enum linecap {
       lc_butt,            /* lj_bevel과 동일, 한쪽에서 다른 쪽으로 직선 연장 */
       lc_round,           /* 반원 */
       lc_square           /* 반경만큼 선을 연장한 다음 연결 */
   };

   typedef struct strokeinfo {
       double radius;
       enum linejoin join;
       enum linecap cap;
       unsigned int calligraphic: 1;
       double penangle;
       double thickness;                   /* 작동하지 않음 */
   } StrokeInfo;

위의 데이터 구조는 경로를 채워진 모양으로 바꾸는 ExpandStroke 루틴에서 사용됩니다. 위의 정보는 해당 루틴에 대한 다양한 컨트롤을 제공합니다. 포스트스크립트에서 예상하는 것과 본질적으로 동일한 의미입니다.

::

   typedef struct ipoint {
       int x;
       int y;
   } IPoint;

정수 점.

::

   typedef struct basepoint {
       double x;
       double y;
   } BasePoint;

이중 점. 이것은 :ref:`SplinePoints <splinefont.SplinePoint>` 및 해당 제어점의 위치를 제공합니다.

::

   typedef struct tpoint {
       double x;
       double y;
       double t;
   } TPoint;

"t" 값이 있는 이중 점. 해당 위치가 스플라인에서 어디에서 발생하는지 나타냅니다. (시작점 자체는 t=0, 끝점은 t=1, 중간 점은 중간 값을 가짐). 새 스플라인을 근사화할 때 사용됩니다.

::

   typedef struct dbounds {
       double minx, maxx;
       double miny, maxy;
   } DBounds;

:ref:`Spline <splinefont.Spline>`, :ref:`SplineChar <splinefont.SplineChar>`, :ref:`RefChar <splinefont.RefChar>`, :ref:`Image <splinefont.ImageList>` 또는 경계 상자가 필요한 다른 모든 것의 경계 상자.

.. code-block:: default
   :name: splinefont.BDFFloat

   typedef struct bdffloat {
       int16 xmin,xmax,ymin,ymax;
       int16 bytes_per_line;
       uint8 *bitmap;
   } BDFFloat;

:ref:`BDFChar <splinefont.BDFChar>`의 부동 선택.

.. code-block:: default
   :name: splinefont.Undoes

   typedef struct undoes {
       struct undoes *next;
       enum undotype { ut_none=0, ut_state, ut_tstate, ut_width,
               ut_bitmap, ut_bitmapsel, ut_composite, ut_multiple } undotype;
       union {
           struct {
               int16 width;
               int16 lbearingchange;
               struct splinepointlist *splines;
               struct refchar *refs;
               struct imagelist *images;
           } state;
           int width;
           struct {
               /*int16 width;*/    /* 너비는 포스트스크립트로 제어해야 함 */
               int16 xmin,xmax,ymin,ymax;
               int16 bytes_per_line;
               uint8 *bitmap;
               BDFFloat *selection;
               int pixelsize;
           } bmpstate;
           struct {                /* 복사본에는 윤곽선 상태와 비트맵 상태 집합이 포함됨 */
               struct undoes *state;
               struct undoes *bitmaps;
           } composite;
           struct {
               struct undoes *mult; /* 복사본에는 여러 하위 복사본(합성, 상태, 너비 등)이 포함됨 */
           } multiple;
       uint8 *bitmap;
       } u;
   } Undoes;

실행 취소 데이터 구조. :ref:`SplineChar <splinefont.SplineChar>` 및 :ref:`BDFChar <splinefont.BDFChar>` 모두에서 사용됩니다. 로컬 클립보드를 포함하는 데도 사용됩니다. 모든 문자 레이어에는 여러 작업을 되돌릴 수 있는 여러 개의 실행 취소(최대 약 10개 정도, 구성 가능)가 있으며, 다음 필드에 함께 연결되어 있습니다(다시 실행도 물론 비슷하게 처리됨).

실행 취소는 여러 유형이 있으며, ut_none은 클립보드가 비어 있을 때만 사용됩니다.

ut_state는 SplineChars에서 사용되며 문자의 한 레이어의 현재 상태 덤프를 포함합니다. 분명히 여기서 많은 것을 최적화할 수 있지만 이것은 쉽습니다. ut_tstate는 ut_state와 동일한 데이터 구조를 가지며 변환 중에 사용되며 디스플레이에 원본과 현재 변환된 것을 모두 그리도록 지시하는 플래그일 뿐입니다(수행한 작업을 볼 수 있도록).

ut_state는 SplineChar 또는 SplineChar의 일부를 복사할 때 클립보드에서도 사용됩니다.

ut_width는 너비(그리고 다른 것은 아님)가 변경될 때 SplineChars에서 사용됩니다.

ut_bitmap은 BDFChars에서 사용되며 비트맵의 현재 상태 덤프입니다. 다시 말하지만 여기에는 최적화의 여지가 있지만 이것은 쉽습니다.

ut_bitmapsel은 BDFChar를 복사할 때 사용됩니다. 선택 항목이 있는 경우(그리고 그것만) bmpstate 구조의 선택 필드에 복사됩니다. 선택 항목이 없으면 전체 비트맵이 :ref:`부동 선택 <splinefont.BDFFloat>`으로 변환되어 선택 필드에 복사됩니다.

ut_composite은 FontView에서 문자의 스플라인과 비트맵을 모두 복사할 때 사용됩니다.

ut_mult는 FontView에서 둘 이상의 문자를 복사할 때 사용됩니다.

.. code-block:: default
   :name: splinefont.BDFChar

   typedef struct bdfchar {
       struct splinechar *sc;
       int16 xmin,xmax,ymin,ymax;
       int16 width;
       int16 bytes_per_line;
       uint8 *bitmap;
       int orig_pos;
       struct bitmapview *views;
       Undoes *undoes;
       Undoes *redoes;
       unsigned int changed: 1;
       unsigned int byte_data: 1;          /* 앤티앨리어싱된 문자의 경우 항목은 흑백 비트가 아닌 회색조 바이트임 */
       BDFFloat *selection;
   } BDFChar;

기본 비트맵 문자 구조. 비트맵을 만드는 데 사용된 SplineChar에 대한 링크가 있습니다. 그런 다음 비트맵의 경계 상자, 문자의 너비(물론 픽셀 단위), 비트맵 배열의 행당 바이트 수, 비트맵을 포함하는 배열에 대한 포인터. 비트맵은 바이트에 8비트가 압축되어 저장되며, 상위 비트가 가장 왼쪽에 있습니다. 모든 행은 새 바이트 경계에서 시작합니다. 각 행에는 (xmax-xmin+1)개의 비트가 있고 각 행에는 (xmax-xmin+8)/8개의 바이트가 있습니다. (ymax-ymin+1)개의 행이 있습니다. 비트 값이 1이면 비트가 그려져야 하고 0이면 투명함을 의미합니다. 그런 다음 현재 글꼴의 인코딩. 이 비트맵을 보는 모든 BitmapView 구조의 연결 목록(따라서 이 비트맵에 대한 모든 변경은 모든 보기에서 다시 그려야 함). 실행 취소 및 다시 실행 세트. 마지막으로 디스크에 저장된 이후 변경되었는지 여부를 나타내는 플래그.

지금까지 비트맵에 대해 이야기했습니다. 바이트맵을 가질 수도 있습니다. 데이터 구조는 각 픽셀이 비트가 아닌 바이트로 표시된다는 점을 제외하고는 정확히 동일합니다. BDFFont에는 이에 대한 clut가 있습니다(모든 문자에 대해 동일함). 하지만 기본적으로 0=>투명, (2^n-1)=>완전히 그려짐, 다른 값은 그 사이의 회색 음영입니다. 1, 2, 4 및 8비트/픽셀의 깊이를 처리할 수 있습니다.

BDFChar의 마지막 것은 (/선택 사항) 부동 선택입니다. 사용자가 선택을 했거나 붙여넣기를 했거나 하는 경우에만 존재합니다.

.. code-block:: default
   :name: splinefont.BDFFont

   typedef struct bdffont {
       struct splinefont *sf;
       int glyphcnt;
       BDFChar **glyphs;            /* charcnt 항목의 배열 */
       int pixelsize;
       int ascent, descent;
       struct bdffont *next;
       struct clut *clut;          /* 앤티앨리어싱된 글꼴에만 해당 */
   } BDFFont;

기본 비트맵 글꼴. 연결된 :ref:`SplineFont <splinefont.SplineFont>`에 대한 참조를 포함합니다. 그런 다음 크기 및 BDFChars 배열(해당 인코딩에 대해 정의된 문자가 없는 경우 배열에 NULL 항목이 있을 수 있음). 그런 다음 글꼴을 다시 인코딩하는 동안 한 루틴 세트에서 사용되는 임시 배열. em-사각형의 픽셀 크기. 어센트 및 디센트(픽셀 단위), 이들은 em-사각형에 합산되어야 합니다. SplineFont의 문자 집합과 일치하는 문자 집합. 이 SplineFont와 관련된 다음 비트맵 글꼴에 대한 포인터.

바이트 글꼴을 다루는 경우 clut도 있습니다. 여기에는 배열의 항목 수와 배열 자체가 포함됩니다. 현재 여기의 항목 수는 항상 16개이지만 변경될 수 있습니다.

.. code-block:: default
   :name: splinefont.SplinePoint

   enum pointtype { pt_curve, pt_corner, pt_tangent };
   typedef struct splinepoint {
       BasePoint me;
       BasePoint nextcp;          /* 제어점 */
       BasePoint prevcp;          /* 제어점 */
       unsigned int nonextcp:1;
       unsigned int noprevcp:1;
       unsigned int nextcpdef:1;
       unsigned int prevcpdef:1;
       unsigned int selected:1;    /* UI용 */
       unsigned int pointtype:2;
       unsigned int isintersection: 1;
       uint16 flex;                /* 이것은 플렉스 세리프이며 icky 플렉스 출력을 거쳐야 함 */
       struct spline *next;
       struct spline *prev;
   } SplinePoint;

SplinePoint는 "me"로 지정된 위치에 있습니다. 다음 :ref:`Spline <splinefont.Spline>`과 관련된 제어점은 nextcp에 위치하고, 이전 Spline과 관련된 제어점은 prevcp에 있습니다. 그런 다음 테스트를 단순화하는 몇 가지 플래그가 있습니다. nextcp가 퇴화된 경우(즉, me와 같은 위치에 있는 경우) nonextcp가 설정되고, prevcp도 마찬가지입니다. 사용자가 제어점을 건드리지 않은 경우 기본값을 가지며, 사용자가 점을 이동하면 fontforge가 제어점을 적절하게 업데이트합니다. 기본값이 아닌 경우 fontforge는 오프셋만 적용합니다.

점이 선택되면 해당 비트가 설정됩니다.

모든 점은 곡선 점, 모서리 점 및 접선 점으로 분류됩니다.

isintersection 비트는 SplineOverlap 루틴 내부에서 사용됩니다. flex 값은 플렉스 힌트용입니다. type1 글꼴에서 읽어온 다음 무시됩니다. 언젠가 사용할 수도 있습니다.

마지막으로 이 점에 연결되는 두 개의 Spline에 대한 포인터가 있습니다.

::

   typedef struct linelist {
       IPoint here;
       struct linelist *next;
   } LineList;

   typedef struct linearapprox {
       double scale;
       unsigned int oneline: 1;
       unsigned int onepoint: 1;
       struct linelist *lines;
       struct linearapprox *next;
   } LinearApprox;

이것들은 그릴 때 :ref:`Spline <splinefont.Spline>`을 근사화하는 데 사용되는 선입니다. 매번 다시 생성할 필요가 없도록 캐시됩니다. 모든 배율에 대해 다른 선 세트가 있습니다(볼 수 있는 세부 사항의 양이 다르기 때문). Spline이 변경되면 해제되고 다시 생성됩니다.

.. code-block:: default
   :name: splinefont.Spline

   typedef struct spline1d {
       double a, b, c, d;
   } Spline1D;

   typedef struct spline {
       unsigned int islinear: 1;
       unsigned int isticked: 1;
       unsigned int isneeded: 1;
       unsigned int isunneeded: 1;
       unsigned int ishorvert: 1;
       unsigned int order2: 1;
       SplinePoint *from, *to;
       Spline1D splines[2];                /* splines[0]는 x 스플라인, splines[1]는 y */
       struct linearapprox *approx;
       /* 가능한 최적화:
           경계 상자 미리 계산
           변곡점 미리 계산
       */
   } Spline;

스플라인은 ``from`` :ref:`SplinePoint <splinefont.SplinePoint>`에서 ``to`` SplinePoint까지 실행됩니다. 스플라인의 두 제어점이 모두 퇴화된 경우 스플라인은 선형입니다(실제로 선형 스플라인으로 이어지는 다른 경우가 있으며, 때로는 이러한 경우가 감지되어 정규 사례로 변환되지만 다른 경우에는 그렇지 않음). 나머지 비트는 다양한 함수에서 스플라인을 처리할 때 사용됩니다. 대부분은 SplineOverlap 루틴에서 사용되지만 일부는 다른 곳에서도 사용됩니다.

Spline1D 구조는 각각 x 및 y 좌표에 대한 방정식을 제공합니다(splines[0]는 x용, splines[1]는 y용).

.. code-block:: default
   :name: splinefont.SplinePointList

   typedef struct splinepointlist {
       SplinePoint *first, *last;
       struct splinepointlist *next;
   } SplinePointList, SplineSet;

SplinePointList(또는 SplineSet)는 :ref:`Spline <splinefont.Spline>` 및 :ref:`SplinePoint <splinefont.SplinePoint>`의 모음입니다. 모든 SplinePoint는 두 개의 Spline(next 및 prev)에 연결할 수 있습니다. 모든 Spline은 두 개의 SplinePoint(from 및 to)에 연결됩니다. SplinePointList는 연결된 경로입니다. 세 가지 경우가 있습니다.

* ``first != last``는 열린 경로가 있음을 의미합니다. 여기서 first->prev==NULL이고 last->next==NULL입니다.
* ``first == last`` 및 :menuselection:`first --> prev==NULL`은 한 점만 있는 퇴화된 경로가 있음을 의미하며, last->next==NULL도 마찬가지입니다.
* ``first == last`` 및 :menuselection:`first --> prev!=NULL`은 닫힌 경로가 있음을 의미합니다. 이것이 가장 일반적인 경우여야 합니다.

일반적으로 일련의 경로는 문자를 구성하며 다음 필드에 함께 연결됩니다.

.. code-block:: default
   :name: splinefont.RefChar

   typedef struct refchar {
       int16 adobe_enc, orig_pos
       int unicode_enc;            /* 붙여넣기에서 사용 */
       double transform[6];        /* 변환 행렬 (3x3 행렬의 첫 두 행, 누락된 행은 0,0,1) */
       SplinePointList *splines;
       struct refchar *next;
       unsigned int checked: 1;
       unsigned int selected: 1;
       DBounds bb;
       struct splinechar *sc;
   } RefChar;

:ref:`SplineChar <splinefont.SplineChar>`에는 다른 문자에 대한 참조가 포함될 수 있습니다. 예를 들어 "Agrave"에는 "A"와 "grave"에 대한 참조가 포함될 수 있습니다. 세 가지 다른 인코딩 값이 있으며, 그 중 orig_pos는 항상 최신 상태가 아닙니다. Adobe_enc는 type1 글꼴에서 seac 명령에서 사용되는 Adobe 표준 인코딩의 위치를 제공합니다. 이것이 -1이면 문자가 adobe 인코딩에 없으므로 Type1 글꼴에 참조를 넣을 수 없습니다(truetype에는 이 제한이 없으며 다른 제한이 있음). 변환 행렬은 표준 포스트스크립트 변환 행렬입니다(2열 3행. 첫 두 행은 표준 회전/크기 조정/뒤집기/기울이기/... 변환을 제공하고 마지막 행은 변환을 제공함). (포스트스크립트와 트루타입 모두 허용되는 변환 종류에 제한이 있음). 스플라인 필드는 참조된 문자를 그리는 빠른 방법을 제공하며, 참조된 문자의 모든 스플라인에 변환 행렬을 적용한 결과입니다. 여러 참조된 문자가 있을 수 있으며 다음 필드에 함께 연결됩니다. 확인된 필드는 루프가 없는지 확인하는 데 사용됩니다(즉, 자신을 참조하는 문자). 선택된 필드는 참조가 선택되었음을 나타냅니다. bb 필드는 변환된 경계 상자를 제공합니다. 그리고 sc 필드는 우리가 참조하는 SplineChar를 가리킵니다.

.. code-block:: default
   :name: splinefont.KernPair

   typedef struct kernpair {
       struct splinechar *sc;
       int off;
       struct kernpair *next;
   } KernPair;

문자가 다른 문자와 커닝되어야 하는 경우 이 구조가 해당 정보를 제공합니다. 모든 SplineChar에는 KernPair의 연결 목록이 첨부되어 있습니다. 해당 목록에서 sc 필드는 쌍의 다른 문자를 나타내고 off는 둘 사이의 오프셋(또는 각각의 왼쪽 및 오른쪽 베어링이 그렇게 해야 한다고 믿게 만드는 것과의 차이)을 정의합니다. Next는 다음 kernpair를 가리킵니다.

.. code-block:: default
   :name: splinefont.Hints

   typedef struct hints {
       double base, width;
       double b1, b2, e1, e2;
       double ab, ae;
       unsigned int adjustb: 1;
       unsigned int adjuste: 1;
       struct hints *next;
   } Hints;

여기서 중요한 필드는 base, width 및 next뿐입니다. 다른 필드는 SplineFill 루틴에서 임시로 사용됩니다. Base는 줄기가 시작되는 위치(x 또는 y 공간)를 제공하고 width는 길이입니다. Width는 음수일 수 있습니다(이 경우 base는 줄기가 끝나는 위치임). Next는 문자의 다음 힌트를 가리킵니다.

.. code-block:: default
   :name: splinefont.ImageList

   typedef struct imagelist {
       struct gimage *image;
       double xoff, yoff;          /* 이미지 왼쪽 상단 모서리의 문자 공간 위치 */
       double xscale, yscale;      /* 이미지의 한 픽셀을 문자 공간의 한 단위로 변환하는 배율 */
       DBounds bb;
       struct imagelist *next;
       unsigned int selected: 1;
   } ImageList;

SplineChars는 배경에 이미지를 가질 수 있습니다(또는 다중 레이어 문자의 경우 전경). 이 구조에는 표시할 이미지에 대한 포인터, 위치 지정 방법 및 크기 조정 방법(이미지에 대한 다른 변환은 지원하지 않음)이 포함됩니다. 경계 상자는 변환이 적용된 후입니다. 다음 필드는 다음 이미지를 가리키고 선택됨은 이 이미지가 선택되었는지 여부를 나타냅니다.

.. code-block:: default
   :name: splinefont.SplineChar

   typedef struct splinechar {
       char *name;
       int enc, unicodeenc;
       int width;
       int16 lsidebearing;         /* type1 글꼴을 읽을 때만 사용 */
       int16 ttf_glyph;            /* ttf 글꼴을 쓸 때만 사용 */
       SplinePointList *splines;
       Hints *hstem;          /* hstem 힌트는 수직 오프셋이 있지만 수평으로 실행됨 */
       Hints *vstem;          /* vstem 힌트는 수평 오프셋이 있지만 수직으로 실행됨 */
       RefChar *refs;
       struct charview *views;   /* 우리를 보는 모든 CharView */
       struct splinefont *parent;
       unsigned int changed: 1;
       unsigned int changedsincelasthhinted: 1;
       unsigned int changedsincelastvhinted: 1;
       unsigned int manualhints: 1;
       unsigned int ticked: 1;     /* 참조 문자 처리용 */
       unsigned int changed_since_autosave: 1;
       unsigned int widthset: 1;   /* emspace 문자가 사라지지 않도록 필요 */
       struct splinecharlist { struct splinechar *sc; struct splinecharlist *next;} *dependents;
               /* 종속 목록은 현재 문자를 직접 참조하는 모든 문자 목록임 */
       SplinePointList *backgroundsplines;
       ImageList *backimages;
       Undoes *undoes[2];
       Undoes *redoes[2];
       KernPair *kerns;
       uint8 *origtype1;           /* 원래 type1(인코딩되지 않은) 문자열 */
       int origlen;                /*  길이. 사용자가 문자를 변경하면 해제됨 */
                                   /*  그때까지는 우리가 이해하지 못하는 힌트 대체와 같은 것을 보존함 */
   } SplineChar;

모든 문자에는 이름이 있습니다(때로는 ".notdef"이지만 있습니다). 현재 글꼴의 위치(인코딩), 유니코드 코드 포인트(유니코드가 아닌 경우 -1일 수 있음). 모든 문자에는 너비가 있습니다. 다음 두 필드는 적절한 형식으로 글꼴을 로드하거나 저장할 때만 의미가 있습니다. lbearing은 seac 명령을 처리하는 데 필요하며 ttf_glyph는 ttf 글꼴을 작성할 때 거의 모든 것에 필요합니다. 스플라인 필드는 모든 전경 경로(:ref:`SplinePointLists <splinefont.SplinePointList>`)를 제공합니다. 수평 및 수직 줄기에 대한 힌트. 여기에 참조된 다른 문자 집합, 다시 전경에만 있습니다. 그런 다음 이 SplineChar를 표시하는 모든 CharView의 연결 목록(이 사람이 변경되면 모두 변경 사항을 반영하도록 업데이트해야 함). 우리를 포함하는 :ref:`SplineFont <splinefont.SplineFont>`에 대한 포인터. 비트 집합: 변경됨은 마지막 디스크 저장 이후 문자가 변경되었음을 의미하고, changedsincelasthhinted는 수평 줄기에 대해 자동 힌트를 실행해야 함을 의미하고, changedsincelastvhinted는 수직 줄기에 대해 실행해야 함을 의미합니다. 수동 힌트는 사용자가 힌트 제공을 제어했음을 의미하며, 명시적으로 요청한 경우에만 자동 힌트를 실행해야 합니다. Ticked는 일반적으로 참조된 문자의 무한 루프를 피하기 위한 임시 필드입니다. changed_since_autosave는 다음에 자동 저장 데이터베이스를 업데이트할 때 이 문자를 작성해야 함을 나타냅니다. Widthset은 사용자가 너비를 변경했음을 의미합니다. 이 비트가 없으면 em 공백에 아무것도 없다고 생각할 수 있습니다(em 공백이 있는 대신).

SplineChar가 다른 문자에서 참조되는 경우 원본을 변경할 때 참조하는 모든 항목도 업데이트해야 합니다(A를 변경하면 Agrave도 다시 표시해야 함).

그런 다음 backgroundplines는 배경 레이어의 SplinePointLists를 나타내고 backimages는 배경 레이어의 이미지를 나타냅니다.

Undoes[0]에는 전경에 대한 실행 취소가 포함되고 undoes[1]에는 배경에 대한 실행 취소가 포함됩니다. 다시 실행도 마찬가지입니다.

마지막으로 kerns는 이 문자 뒤에 오는 특수 문자에 대한 커닝 데이터 목록을 제공합니다. 예를 들어 "VA" 조합에는 커닝이 필요할 수 있으며, "V"를 나타내는 SplineChar에는 "A"에 대한 데이터가 있는 :ref:`KernPair <splinefont.KernPair>`에 대한 포인터가 있습니다.

.. code-block:: default
   :name: splinefont.SplineFont

   typedef struct splinefont {
       char *fontname, *fullname, *familyname, *weight;
       char *copyright;
       char *filename;
       char *version;
       double italicangle, upos, uwidth;
       int ascent, descent;
       int glyphcnt;
       SplineChar **glyphs;
       unsigned int changed: 1;
       unsigned int changed_since_autosave: 1;
       unsigned int display_antialias: 1;
       unsigned int dotlesswarn: 1;                /* 사용자는 글꼴에 점 없는 i 문자가 없다고 경고 받음 */
       /*unsigned int wasbinary: 1;*/
       struct fontview *fv;
       enum charset encoding_name;
       SplinePointList *gridsplines;
       Undoes *gundoes, *gredoes;
       BDFFont *bitmaps;
       char *origname;             /* 글꼴 파일의 파일 이름 (즉, sfd가 아닌 경우) */
       char *autosavename;
       int display_size;
   } SplineFont;

처음 네 개의 이름은 저작권, 버전, 이탤릭 각도, 밑줄 위치, 밑줄 두께와 마찬가지로 PostScript에서 직접 가져온 것입니다. 어센트와 디센트는 함께 em-사각형을 만듭니다. 일반적으로 이것은 1000이지만 원하는 경우 변경할 수 있습니다.

Charcnt는 chars 배열의 크기를 나타냅니다. 배열에 구멍(즉, NULL 값)이 있을 수 있습니다.

Changed는 마지막으로 파일을 저장한 이후 일부 문자, 비트맵, 메트릭, 무언가가 변경되었음을 나타냅니다. changed_since_autosave는 자동 저장이 마지막으로 발생한 이후 무언가가 변경되었음을 의미합니다(따라서 다음에 자동 저장이 발생할 때 이 글꼴을 실제로 처리해야 함).

Display_antialias는 FontView에 비트맵 글꼴 대신 앤티앨리어싱 바이트맵 글꼴을 표시하고 있음을 의미합니다. 이것들은 더 좋아 보이지만 느립니다.

Dotlesswarn은 i 또는 j를 기반으로 악센트 부호를 만들려고 할 때 글꼴에 점 없는 버전의 해당 문자가 없다고 사용자에게 경고했음을 의미합니다(다시 경고할 필요가 없음). 작업은 점 있는 버전으로 진행되었습니다.

글꼴과 관련된 FontView는 하나뿐입니다(다른 데이터 구조는 여러 보기를 허용했지만 글꼴은 그렇지 않음).

글꼴에는 문자 집합과 인코딩이 있습니다.

Gridsplines는 문자의 배경 그리드로 표시되는 모든 스플라인에 대한 :ref:`SplinePointLists <splinefont.SplinePointList>`를 포함합니다. 그리고 gundoes와 gredoes는 그것들과 관련된 실행 취소/다시 실행입니다.

Bitmaps는 이 SplineFont에 대해 생성된 모든 비트맵을 포함합니다.

Origname은 이것을 얻기 위해 읽은 파일의 이름을 포함합니다. 위의 Filename은 이 글꼴과 관련된 .sfd 파일의 이름을 포함하지만 origname은 임의의 포스트스크립트 또는 트루타입 글꼴도 포함할 수 있습니다.

Autosavename은 현재 충돌 복구 내용을 디스크에 저장하는 데 사용되는 이름을 나타냅니다.

Display_size는 FontView에 글꼴을 얼마나 크게 표시할지를 나타냅니다.


함수 선언
---------------------

::

   struct fontdict;         /* 다음 네 개의 데이터 구조는 psfont.h에 있음 */
   struct chars;
   struct findsel;
   struct charprocs;
   enum charset;                   /* charset.h에 있음 */

   extern SplineFont *SplineFontFromPSFont(struct fontdict *fd);
   extern bool SFIsFixedWidth(SplineFont *sf);
   extern struct chars *SplineFont2Chrs(SplineFont *sf, int round);
   enum fontformat { ff_pfa, ff_pfb, ff_ptype3, ff_ptype0, ff_ttf, ff_none };
   extern int WritePSFont(char *fontname,SplineFont *sf,enum fontformat format);
   extern int WriteTTFFont(char *fontname,SplineFont *sf);
   int SFReencodeFont(SplineFont *sf,enum charset new_map);

   extern int DoubleNear(double a,double b);
   extern int DoubleNearish(double a,double b);

   extern void LineListFree(LineList *ll);
   extern void LinearApproxFree(LinearApprox *la);
   extern void SplineFree(Spline *spline);
   extern void SplinePointFree(SplinePoint *sp);
   extern void SplinePointListFree(SplinePointList *spl);
   extern void SplinePointListsFree(SplinePointList *head);
   extern void RefCharFree(RefChar *ref);
   extern void RefCharsFree(RefChar *ref);
   extern void UndoesFree(Undoes *undo);
   extern void HintsFree(Hints *h);
   extern Hints *HintsCopy(Hints *h);
   extern SplineChar *SplineCharCopy(SplineChar *sc);
   extern BDFChar *BDFCharCopy(BDFChar *bc);
   extern void ImageListsFree(ImageList *imgs);
   extern void SplineCharFree(SplineChar *sc);
   extern void SplineFontFree(SplineFont *sf);
   extern void SplineRefigure(Spline *spline);
   extern Spline *SplineMake(SplinePoint *from, SplinePoint *to);
   extern LinearApprox *SplineApproximate(Spline *spline, double scale);
   extern int SplinePointListIsClockwise(SplineSet *spl);
   extern void SplineSetFindBounds(SplinePointList *spl, DBounds *bounds);
   extern void SplineCharFindBounds(SplineChar *sc,DBounds *bounds);
   extern void SplineFontFindBounds(SplineFont *sf,DBounds *bounds);
   extern void SplinePointCategorize(SplinePoint *sp);
   extern void SCCategorizePoints(SplineChar *sc);
   extern SplinePointList *SplinePointListCopy(SplinePointList *base);
   extern SplinePointList *SplinePointListCopySelected(SplinePointList *base);
   extern SplinePointList *SplinePointListTransform(SplinePointList *base, double transform[6], int allpoints );
   extern SplinePointList *SplinePointListShift(SplinePointList *base, double xoff, int allpoints );
   extern SplinePointList *SplinePointListRemoveSelected(SplinePointList *base);
   extern void SplinePointListSet(SplinePointList *tobase, SplinePointList *frombase);
   extern void SplinePointListSelect(SplinePointList *spl,int sel);
   extern void SCRefToSplines(SplineChar *sc,RefChar *rf);
   extern void SCReinstanciateRefChar(SplineChar *sc,RefChar *rf);
   extern void SCReinstanciateRef(SplineChar *sc,SplineChar *rsc);
   extern void SCRemoveDependent(SplineChar *dependent,RefChar *rf);
   extern void SCRemoveDependents(SplineChar *dependent);
   extern RefChar *SCCanonicalRefs(SplineChar *sc, int isps);
   extern void BCCompressBitmap(BDFChar *bdfc);
   extern void BCRegularizeBitmap(BDFChar *bdfc);
   extern void BCPasteInto(BDFChar *bc,BDFChar *rbc,int ixoff,int iyoff, int invert);
   extern BDFChar *SplineCharRasterize(SplineChar *sc, int pixelsize);
   extern BDFFont *SplineFontRasterize(SplineFont *sf, int pixelsize);
   extern BDFChar *SplineCharAntiAlias(SplineChar *sc, int pixelsize,int linear_scale);
   extern BDFFont *SplineFontAntiAlias(SplineFont *sf, int pixelsize,int linear_scale);
   extern void BDFCharFree(BDFChar *bdfc);
   extern void BDFFontFree(BDFFont *bdf);
   extern void BDFFontDump(char *filename,BDFFont *font, char *encodingname);
   extern double SplineSolve(Spline1D *sp, double tmin, double tmax, double sought_y, double err);
   extern void SplineFindInflections(Spline1D *sp, double *_t1, double *_t2 );
   extern int NearSpline(struct findsel *fs, Spline *spline);
   extern double SplineNearPoint(Spline *spline, BasePoint *bp, double fudge);
   extern void SCMakeDependent(SplineChar *dependent,SplineChar *base);
   extern SplinePoint *SplineBisect(Spline *spline, double t);
   extern Spline *ApproximateSplineFromPoints(SplinePoint *from, SplinePoint *to,
           TPoint *mid, int cnt);
   extern int SplineIsLinear(Spline *spline);
   extern int SplineInSplineSet(Spline *spline, SplineSet *spl);
   extern void SplineCharMerge(SplineSet **head);
   extern void SplineCharSimplify(SplineSet *head);
   extern void SplineCharRemoveTiny(SplineSet *head);
   extern SplineFont *SplineFontNew(void);
   extern SplineFont *SplineFontBlank(int enc,int charcnt);
   extern SplineSet *SplineSetReverse(SplineSet *spl);
   extern SplineSet *SplineSetsExtractOpen(SplineSet **tbase);
   extern SplineSet *SplineSetsCorrect(SplineSet *base);
   extern void SPAverageCps(SplinePoint *sp);
   extern void SplineCharDefaultPrevCP(SplinePoint *base, SplinePoint *prev);
   extern void SplineCharDefaultNextCP(SplinePoint *base, SplinePoint *next);
   extern void SplineCharTangentNextCP(SplinePoint *sp);
   extern void SplineCharTangentPrevCP(SplinePoint *sp);
   extern int PointListIsSelected(SplinePointList *spl);
   extern void SplineSetsUntick(SplineSet *spl);
   extern void SFOrderBitmapList(SplineFont *sf);

   extern SplineSet *SplineSetStroke(SplineSet *spl,StrokeInfo *si);
   extern SplineSet *SplineSetRemoveOverlap(SplineSet *base);

   extern void FindBlues( SplineFont *sf, double blues[14], double otherblues[10]);
   extern void FindHStems( SplineFont *sf, double snaps[12], double cnt[12]);
   extern void FindVStems( SplineFont *sf, double snaps[12], double cnt[12]);
   extern void SplineCharAutoHint( SplineChar *sc);
   extern void SplineFontAutoHint( SplineFont *sf);
   extern int AfmSplineFont(FILE *afm, SplineFont *sf,int type0);
   extern char *EncodingName(int map);

   extern int SFDWrite(char *filename,SplineFont *sf);
   extern int SFDWriteBak(SplineFont *sf);
   extern SplineFont *SFDRead(char *filename);
   extern SplineFont *SFReadTTF(char *filename);
   extern SplineFont *LoadSplineFont(char *filename);
   extern SplineFont *ReadSplineFont(char *filename);      /* 이것을 사용하지 말고 LoadSF를 사용하십시오. */

   extern SplineChar *SCBuildDummy(SplineChar *dummy,SplineFont *sf,int i);
   extern SplineChar *SFMakeChar(SplineFont *sf,int i);
   extern BDFChar *BDFMakeChar(BDFFont *bdf,int i);

   extern void SCUndoSetLBearingChange(SplineChar *sc,int lb);
   extern Undoes *SCPreserveState(SplineChar *sc);
   extern Undoes *SCPreserveWidth(SplineChar *sc);
   extern Undoes *BCPreserveState(BDFChar *bc);
   extern void BCDoRedo(BDFChar *bc,struct fontview *fv);
   extern void BCDoUndo(BDFChar *bc,struct fontview *fv);

   extern int SFIsCompositBuildable(SplineFont *sf,int unicodeenc);
   extern void SCBuildComposit(SplineFont *sf, SplineChar *sc, int copybmp,
           struct fontview *fv);

   extern void BDFFontDump(char *filename,BDFFont *font, char *encodingname);
   extern int getAdobeEnc(char *name);

   extern SplinePointList *SplinePointListInterpretPS(FILE *ps);
   extern void PSFontInterpretPS(FILE *ps,struct charprocs *cp);

   extern int SFFindChar(SplineFont *sf, int unienc, char *name );

   extern char *getFontForgeDir(char *buffer);
   extern void DoAutoSaves(void);
   extern int DoAutoRecovery(void);
   extern SplineFont *SFRecoverFile(char *autosavename);
   extern void SFAutoSave(SplineFont *sf);
   extern void SFClearAutoSave(SplineFont *sf);
   #endif
