X11 PCF 비트맵 글꼴 파일 형식
===================================

이 파일은 `X11 소스 <http://ftp.x.org/pub/R6.4/xc/lib/font/bitmap/>`__를 기반으로 합니다. 여기에 있는 내용이 소스에 있는 내용과 다른 경우 제 진술은 올바르지 않습니다. 소스가 최종적입니다.

pcf(portable compiled format) 파일 형식은 XServer에서 사용하는 비트맵 글꼴의 이진 표현입니다. 파일 헤더와 일련의 테이블로 구성되며, 헤더에는 모든 테이블에 대한 포인터가 포함되어 있습니다.


파일 헤더
-----------

파일 헤더에는 가장 낮은 유효 바이트가 먼저 저장된 32비트 정수가 포함되어 있습니다.

.. highlight:: c

::

   char header[4];                  /* 항상 "\1fcp" */
   lsbint32 table_count;
   struct toc_entry {
       lsbint32 type;              /* 아래 참조, 어떤 테이블인지 나타냄 */
       lsbint32 format;            /* 아래 참조, 테이블의 데이터 형식을 나타냄 */
       lsbint32 size;              /* 바이트 단위 */
       lsbint32 offset;            /* 파일 시작부터 */
   } tables[table_count];

type 필드는 다음 중 하나일 수 있습니다.

::

   #define PCF_PROPERTIES               (1<<0)
   #define PCF_ACCELERATORS            (1<<1)
   #define PCF_METRICS                 (1<<2)
   #define PCF_BITMAPS                 (1<<3)
   #define PCF_INK_METRICS             (1<<4)
   #define PCF_BDF_ENCODINGS           (1<<5)
   #define PCF_SWIDTHS                 (1<<6)
   #define PCF_GLYPH_NAMES             (1<<7)
   #define PCF_BDF_ACCELERATORS        (1<<8)

format 필드는 다음 중 하나일 수 있습니다.

::

   #define PCF_DEFAULT_FORMAT       0x00000000
   #define PCF_INKBOUNDS           0x00000200
   #define PCF_ACCEL_W_INKBOUNDS   0x00000100
   #define PCF_COMPRESSED_METRICS  0x00000100

format 필드는 다음으로 수정될 수 있습니다.

::

   #define PCF_GLYPH_PAD_MASK       (3<<0)            /* 설명은 비트맵 테이블 참조 */
   #define PCF_BYTE_MASK           (1<<2)            /* 설정된 경우 최상위 바이트가 먼저 */
   #define PCF_BIT_MASK            (1<<3)            /* 설정된 경우 최상위 비트가 먼저 */
   #define PCF_SCAN_UNIT_MASK      (3<<4)            /* 설명은 비트맵 테이블 참조 */

형식은 각 테이블의 첫 번째 단어로 반복됩니다. 대부분의 테이블은 하나의 형식(기본값)만 가지지만 일부는 대체 형식을 가집니다. 형식의 상위 3바이트는 전체 형식을 설명합니다. 정수와 비트가 저장되는 방식은 위의 마스크 비트 중 하나를 추가하여 변경할 수 있습니다.

모든 테이블은 32비트 경계에서 시작합니다(그리고 0으로 채워짐).


속성 테이블
----------------

::

   lsbint32 format;                 /* 항상 가장 낮은 유효 바이트가 먼저 저장됨! */
   int32 nprops;                   /* 형식에 지정된 바이트 순서로 저장됨 */
   struct props {
       int32 name_offset;          /* 다음 문자열 테이블로의 오프셋 */
       int8 isStringProp;
       int32 value;                /* 정수 속성의 값, 문자열 속성의 오프셋 */
   } props[nprops];
   char padding[(nprops&3)==0?0:(4-(nprops&3))];   /* 다음 int32 경계까지 채움 */
   int string_size;                /* 모든 문자열의 총 크기 (종료 NUL 포함) */
   char strings[string_size];
   char padding2[];

이러한 속성은 X가 사용자에게 제공하는 글꼴 원자입니다. 많은 속성은 `xc/doc/specs/XLFD/xlfd.tbl.ms <http://ftp.x.org/pub/R6.4/xc/doc/specs/XLFD/xlfd.tbl.ms>`__ 또는 `여기 <http://rpmfind.net/linux/RPM/redhat/6.2/i386/XFree86-doc-3.3.6-20.i386.html>`__에서 설명합니다(X 프로토콜은 이러한 원자를 제한하지 않으므로 일부 글꼴에 대해 다른 원자가 정의될 수 있음).

속성 이름을 찾으려면: ``strings + props[i].name_offset``


.. _pcf-format.MetricsData:

메트릭 데이터
------------

여러 테이블(PCF_METRICS, PCF_INK_METRICS 및 가속기 테이블 내)에는 압축(PCF_COMPRESSED_METRICS) 또는 비압축(DEFAULT) 형식일 수 있는 메트릭 데이터가 포함되어 있습니다. 압축 형식은 값을 포함하기 위해 바이트를 사용하고 비압축 형식은 쇼트를 사용합니다. (압축된) 바이트는 0x80으로 오프셋된 부호 없는 바이트입니다(따라서 실제 값은 ``(getc(pcf_file)-0x80)``이 됨). 데이터는 다음과 같이 저장됩니다.

압축됨

::

   uint8 left_sided_bearing;
   uint8 right_side_bearing;
   uint8 character_width;
   uint8 character_ascent;
   uint8 character_descent;
    /* 암시적 문자 속성 필드 = 0 */

비압축

::

   int16 left_sided_bearing;
   int16 right_side_bearing;
   int16 character_width;
   int16 character_ascent;
   int16 character_descent;
   uint16 character_attributes;

이것은 XCharStruct에 필요한 데이터를 제공합니다.


가속기 테이블
------------------

이 데이터는 글꼴 전체에 대한 다양한 정보를 제공합니다. 이 데이터 구조는 두 테이블 PCF_ACCELERATORS 및 PCF_BDF_ACCELERATORS에서 사용됩니다. 테이블은 DEFAULT 형식 또는 PCF_ACCEL_W_INKBOUNDS 형식일 수 있습니다(이 경우 끝에 추가 메트릭 데이터가 있음).

가속기 테이블은 다음과 같습니다.

::

   lsbint32 format;                 /* 항상 가장 낮은 유효 바이트가 먼저 저장됨! */
   uint8 noOverlap;                /* 모든 i에 대해 max(metrics[i].rightSideBearing - metrics[i].characterWidth) */
                                   /*      <= minbounds.leftSideBearing */
   uint8 constantMetrics;          /* XFontStruct의 perchar 필드가 NULL일 수 있음을 의미함 */
   uint8 terminalFont;             /* constantMetrics가 참이고 모든 문자에 대해: */
                                   /*      왼쪽 사이드 베어링==0 */
                                   /*      오른쪽 사이드 베어링== 문자의 너비 */
                                   /*      문자의 어센트==글꼴의 어센트 */
                                   /*      문자의 디센트==글꼴의 디센트 */
   uint8 constantWidth;            /* 택배와 같은 고정 폭 글꼴 */
   uint8 inkInside;                /* 모든 잉크 비트가 [0,charwidth] 사이의 x와 */
                                   /*  [-descent,ascent] 사이의 y를 가진 사각형 내에 있음을 의미함. 따라서 그릴 때 다른 문자와 잉크가 겹치지 않음 */
   uint8 inkMetrics;               /* 잉크 메트릭이 어딘가에서 메트릭과 다른 경우 참 */
   uint8 drawDirection;            /* 0=>왼쪽에서 오른쪽으로, 1=>오른쪽에서 왼쪽으로 */
   uint8 padding;
   int32 fontAscent;               /* 형식에 지정된 바이트 순서 */
   int32 fontDescent;
   int32 maxOverlap;               /* ??? */
   Uncompressed_Metrics minbounds;
   Uncompressed_Metrics maxbounds;
   /* 형식이 PCF_ACCEL_W_INKBOUNDS인 경우 다음 필드를 포함 */
       Uncompressed_Metrics ink_minbounds;
       Uncompressed_Metrics ink_maxbounds;
   /* 그렇지 않으면 해당 필드는 파일에 없으며 위의 min/maxbounds를 복제하여 채워야 함 */

두 테이블이 모두 있는 경우 일반 가속기보다 BDF 가속기를 선호해야 합니다. BDF 가속기에는 글꼴의 인코딩된 문자만 참조하는 데이터가 포함되어 있습니다(단순 가속기 테이블에는 모든 글리프가 포함됨). 따라서 BDF 가속기가 더 정확합니다.


메트릭 테이블
--------------

두 가지 다른 메트릭 테이블이 있습니다. PCF_METRICS와 PCF_INK_METRICS. 전자는 저장된 비트맵의 크기를 포함하고 후자는 최소 경계 상자를 포함합니다. 두 테이블은 동일한 데이터를 포함할 수 있지만 많은 CJK 글꼴은 비트맵을 채워서 모든 비트맵이 동일한 크기가 되도록 합니다. 테이블 형식은 DEFAULT 또는 PCF_COMPRESSED_METRICS일 수 있습니다(설명은 :ref:`메트릭 데이터 <pcf-format.MetricsData>` 섹션 참조).

::

   lsbint32 format;                 /* 항상 가장 낮은 유효 바이트가 먼저 저장됨! */
   /* 형식이 압축된 경우 */
       int16 metrics_count;
       Compressed_Metrics metrics[metrics_count];
   /* 그렇지 않고 형식이 기본값(비압축)인 경우 */
       int32 metrics_count;
       Uncompressed_Metrics metrics[metrics_count];
   /* endif */


비트맵 테이블
----------------

비트맵 테이블의 유형은 PCF_BITMAPS입니다. 형식은 PCF_DEFAULT여야 합니다.

::

   lsbint32 format;                 /* 항상 가장 낮은 유효 바이트가 먼저 저장됨! */
   int32 glyph_count;              /* 바이트 순서는 형식에 따라 다르며 메트릭 수와 동일해야 함 */
   int32 offsets[glyph_count];     /* 비트맵 데이터에 대한 바이트 오프셋 */
   int32 bitmapSizes[4];           /* 다양한 패딩 옵션에 따라 비트맵 데이터가 차지할 크기 */
                                   /*  파일에서 실제로 사용되는 것은 (format&3)으로 지정됨 */
   char bitmap_data[bitmapsizes[format&3]];    /* 비트맵 데이터. 형식에는 다음을 나타내는 플래그가 포함됨: */
                                   /* 바이트 순서 (format&4 => LSB가 먼저)*/
                                   /* 비트 순서 (format&8 => LSB가 먼저) */
                                   /* 각 글리프의 비트맵에서 각 행이 채워지는 방식 (format&3) */
                                   /*  0=>바이트, 1=>쇼트, 2=>정수 */
                                   /* 비트가 저장되는 방식 (바이트, 쇼트, 정수) (format>>4)&3 */
                                   /*  0=>바이트, 1=>쇼트, 2=>정수 */


인코딩 테이블
------------------

인코딩 테이블의 유형은 PCF_BDF_ENCODINGS입니다. 형식은 PCF_DEFAULT여야 합니다.

::

   lsbint32 format;                 /* 항상 가장 낮은 유효 바이트가 먼저 저장됨! */
   int16 min_char_or_byte2;        /* XFontStruct에서와 같이 */
   int16 max_char_or_byte2;        /* XFontStruct에서와 같이 */
   int16 min_byte1;                /* XFontStruct에서와 같이 */
   int16 max_byte1;                /* XFontStruct에서와 같이 */
   int16 default_char;             /* XFontStruct에서와 같이 */
   int16 glyphindeces[(max_char_or_byte2-min_char_or_byte2+1)*(max_byte1-min_byte1+1)];
                                   /* 각 인코딩 값에 해당하는 글리프 인덱스를 제공함 */
                                   /* 값 0xffff는 해당 인코딩에 대한 글리프가 없음을 의미함 */

단일 바이트 인코딩의 경우 min_byte1==max_byte1==0이고 인코딩된 값은 [min_char_or_byte2,max_char_or_byte2] 사이입니다. 인코딩에 해당하는 글리프 인덱스는 glyphindex[encoding-min_char_or_byte2]입니다.

그렇지 않으면 [min_byte1,max_byte1]은 2바이트 인코딩의 첫 번째(상위) 바이트에 허용되는 범위를 지정하고, [min_char_or_byte2,max_char_or_byte2]는 두 번째 바이트의 범위입니다. 2바이트 인코딩(enc1,enc2)에 해당하는 글리프 인덱스는 glyph_index[(enc1-min_byte1)*(max_char_or_byte2-min_char_or_byte2+1)+ enc2-min_char_or_byte2]입니다.

모든 글리프가 인코딩될 필요는 없습니다. 모든 인코딩이 글리프와 연결될 필요는 없습니다.


확장 가능한 너비 테이블
-------------------------

인코딩 테이블의 유형은 PCF_SWIDTHS입니다. 형식은 PCF_DEFAULT여야 합니다.

::

   lsbint32 format;                 /* 항상 가장 낮은 유효 바이트가 먼저 저장됨! */
   int32 glyph_count;              /* 바이트 순서는 형식에 따라 다르며 메트릭 수와 동일해야 함 */
   int32 swidths[glyph_count];     /* 비트맵 데이터에 대한 바이트 오프셋 */

문자의 확장 가능한 너비는 em 단위(em의 1/1000)로 된 해당 포스트스크립트 문자의 너비입니다.


글리프 이름 테이블
---------------------

인코딩 테이블의 유형은 PCF_GLYPH_NAMES입니다. 형식은 PCF_DEFAULT여야 합니다.

::

   lsbint32 format;                 /* 항상 가장 낮은 유효 바이트가 먼저 저장됨! */
   int32 glyph_count;              /* 바이트 순서는 형식에 따라 다르며 메트릭 수와 동일해야 함 */
   int32 offsets[glyph_count];     /* 문자열 데이터에 대한 바이트 오프셋 */
   int32 string_size;
   char string[string_size];

각 문자와 관련된 포스트스크립트 이름.

--------------------------------------------------------------------------------

기타 정보 출처:

* http://www.tsg.ne.jp/GANA/S/pcf2bdf/pcf.pdf
* http://myhome.hananet.net/~bumchul/xfont/pcf.txt
