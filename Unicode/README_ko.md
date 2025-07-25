# FontForge용 유니코드 지원

## 생성된 코드 업데이트

재생성하려면 다음을 실행하십시오.

```bash
python makeutype.py
```

이를 실행하려면 Python 3.7 이상이 필요합니다. 코드를 생성하는 데 필요한 모든 파일을 unicode.org에서 다운로드합니다.

최신 버전의 유니코드로 업데이트하려면 실행하기 전에 `makeutype.py`에서 `UNIDATA_VERSION`을 업데이트하십시오. 레거시상의 이유로 [`data/makeutypedata.py`](data/makeutypedata.py)에는 수동으로 정의된 속성이 추가로 포함되어 있으며, 원하는 대로 업데이트할 수도 있습니다. 그러나 일반적으로 수동 개입은 필요하지도 바람직하지도 않습니다. 대신 모든 것이 유니코드 데이터 파일에서 제공되어야 합니다.

`makeutype.py`는 [black](https://github.com/psf/black)으로 자동 서식 지정됩니다.

## unialt.c

이 파일에는 문자에 대한 NFKD 시퀀스와 수동으로 정의된 '시각적' 대안(기본적으로 동형이의어)을 모두 제공하는 테이블이 포함되어 있습니다. 문자에 NFKD 시퀀스가 있는 경우 해당 시퀀스가 우선 적용되며 시각적 대안은 제공되지 않습니다. `isdecompositonnormative`를 사용하여 NFKD 시퀀스와 시각적 대안을 구별할 수 있습니다.

수동으로 정의된 대안은 [`data/makeutypedata.py`](data/makeutypedata.py)의 `VISUAL_ALTS`에서 설정됩니다.

## uninames.c

이 파일에는 유니코드 NamesList.txt에서 정의된 대로 문자의 이름 및 주석에 대한 정보를 제공하는 함수가 포함되어 있습니다. 또한 `unicoderanges.c`에서 사용하는 블록 및 평면에 대한 정보도 제공합니다.

공간을 절약하기 위해 이름과 주석은 간단한 사전 코딩 체계를 사용하여 압축됩니다. 다음과 같이 작동합니다.

* `lexicon_data`에는 일반적으로 발생하는 단어 시퀀스가 포함됩니다. 한 단어 시퀀스의 마지막 문자에는 상위 비트가 설정됩니다. 결과적으로 이 데이터 구조는 ASCII 단어 시퀀스만 보유할 수 있습니다.
* `lexicon_offset`(`lexicon_shift`와 함께)은 지정된 시퀀스에 대한 `lexicon_data`의 시작 위치를 결정합니다.
* `phrasebook_data`에는 사전에서 일반적으로 발생하는 단어 시퀀스를 관련 사전 항목에 대한 인덱스로 대체한 NamesList 데이터가 포함됩니다.
  - 이 배열의 데이터는 UTF-8로 인코딩되어야 합니다.
  - 사전 인덱스는 상위 비트가 설정된 2바이트 시퀀스로 인코딩됩니다. 0b10xxxxxx 0b1xxxxxxx. 다른 유효한 UTF-8 시퀀스와 구별할 수 있도록 선택되었으며 인덱싱에 13개의 사용 가능한 비트를 제공합니다.
  - 항목은 null로 끝납니다.
  - 주석 줄의 첫 번째 문자는 사전 대체에서 명시적으로 제외되어 쉽게 '예쁘게' 만들 수 있습니다.
  - 줄 바꿈도 대체에서 제외되므로 주석으로 쉽게 건너뛸 수 있습니다.
* `phrasebook_offset`(`phrasebook_shift`와 함께)은 지정된 문자에 대한 `phrasebook_data`의 시작 위치를 결정합니다.


## utype.c

이 파일에는 유니코드 유형 정보(islower/isupper/istitle 등) 및 대소문자 변환 유틸리티(upper/lower/title case + tomirror)가 포함되어 있습니다.

유형 정보는 주로 UnicodeData.txt에 정의된 속성에서 파생됩니다.

이 파일에는 '포즈' 정보도 포함되어 있습니다. 여기에는 정식 결합 클래스(실제로 직접 사용되지는 않음)와 마크를 배치하는 방법을 정의하려는 휴리스틱 속성이 포함됩니다. 특히 '악센트 부호가 있는 글리프 빌드' 기능에서 사용됩니다.

포즈 정보는 역사적으로 수동으로 정의되었습니다(이전 코드 생성 체계에서는 [combiners.h](https://github.com/fontforge/fontforge/blob/892cd25ae1d8c78dbbaba5c7426112d761ff6c3b/Unicode/combiners.h)에서). 이는 [`data/makeutypedata.py`](data/makeutypedata.py)에 있는 `MANUAL_POSES` 매핑으로 존재합니다.

정식 결합 클래스와 포즈는 독립적인 개념이며 실제로 서로 바꿔 사용할 수 없습니다. 그러나 일반적으로 상관 관계가 있으며, 최선의 추측으로 수동 포즈가 정의되지 않은 경우 전자에서 추론하려고 시도합니다.
