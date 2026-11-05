# sishi-zhiming — 前四史 ‘知名’ 용례 콘코던스

논문 「前四史 사관 서술에 나타난 ‘知名’의 분포 양상과 정형구화」(박수민, 《中國文學》, 한국중국어문학회, 2026)의 주석 자료입니다. 中國 前四史(《史記》·《漢書》·《後漢書》·《三國志》 裴松之 注 포함)를 중심으로 선진부터 위진까지의 문헌에서 검색 문자열 ‘知名’의 전 용례 198건을 모아 구성성분 여부, 구 유형, 출전, 성립 연대, 전승 관계, 발화·인용 여부, 공기 요소를 주석한 것입니다.

*Supplementary data for “Zhiming 知名 in the First Four Standard Histories: Distribution and Formulaic Use in the Historians’ Narrative” (Park, Sumin, Chinese Literature 中國文學, 2026): a KWIC concordance of all 198 occurrences of the string 知名 in pre-Qin to Wei–Jin texts, chiefly the First Four Standard Histories, annotated for constituency, phrase type, source, date of composition, textual transmission, quoted-speech status, and co-occurring elements.*

## 파일

| 파일 | 내용 |
|---|---|
| `appendix-zhiming.xlsx` | 논문 2.3의 각주가 가리키는 주석 자료를 한 통합문서로 묶은 것. 시트: 설명(시트 목록·표기 규약·표준 필터 검산 수식) / 컬럼 정의 / 콘코던스(198) / 裴注 인용 문헌(32) / 後漢書 주어 시기(57) / 史記 有名(7). 아래 CSV와 같은 내용이며, 검산 수식으로 논문의 건수(198 → 187 → 173 → 168 → 158 → 145)를 직접 확인할 수 있다. |
| `data/concordance.csv` | 콘코던스 198행. KWIC 3분할(`preceding`·`target`·`following`)과 주석 22열. |
| `data/peizhu_source_genre.csv` | 《三國志》 裴注가 인용한 문헌 32종의 《隋書·經籍志》·《舊唐書·經籍志》 분류와 분석용 묶음(`genre_group`). `peizhu_source`를 키로 콘코던스에 병합한다. |
| `data/hhs_subject_period.csv` | 《後漢書》 서술 층위 술어 57건의 주어 인물과 주된 활동 시기(桓帝 즉위 146년 기준). `id`를 키로 병합한다. |
| `data/youming_shiji.csv` | 비교 자료: 《史記》의 검색 문자열 ‘有名’ 7건. |
| `data/codebook.md` | 컬럼 정의·값·표기 규약·분석용 표준 필터·알려진 한계. |
| `LICENSE` | CC0 1.0. |

CSV는 UTF-8, 쉼표 구분, 첫 행이 열 이름입니다. 빈 셀은 ‘해당 없음’을 뜻합니다(미상이 아님). 한자는 원문의 정체자 그대로이며, 원문은 Chinese Text Project(https://ctext.org)의 전자 텍스트에서 가져왔습니다(2026-07 검색; 선진·양한 114건 + 三國志 84건). 연구용 원본에 있던 판정 근거 메모 열(`note`)은 내부 메모와 URL이 섞여 있어 공개판에서 뺐습니다.

## 표준 필터(논문의 집계 기준)

| 단계 | 건수 | 조건 |
|---|--:|---|
| 검색된 용례 전체 | 198 | 모든 행 |
| 구성성분 성립 | 187 | `is_phrase = TRUE` |
| 前四史 | 173 | + `book` ∈ {史記, 漢書, 後漢書, 三國志} |
| 축자 전재 제외 | 168 | + `parallel_type ≠ verbatim` |
| 서술 층위 | 158 | + `is_quote = FALSE` |
| 서술 층위 술어 | 145 | + `phrase_type = predicative` |

《三國志》 본문(陳壽)과 裴注는 `peizhu_source`의 공란 여부로 구분합니다. pandas로 읽을 때는 빈 셀이 NaN이 되지 않도록 `pd.read_csv(..., keep_default_na=False)`를 권합니다.

```python
import pandas as pd
df = pd.read_csv('data/concordance.csv', keep_default_na=False, dtype=str)
narr = df[(df.is_phrase == 'TRUE') & df.book.isin(['史記', '漢書', '後漢書', '三國志'])
          & (df.parallel_type != 'verbatim') & (df.is_quote == 'FALSE')]
print(len(narr))   # 158
```

## 인용

자료를 쓰실 때는 논문과 이 저장소를 함께 인용해 주십시오(`CITATION.cff` 참조).

- 박수민(2026), 〈前四史 사관 서술에 나타난 ‘知名’의 분포 양상과 정형구화〉, 《中國文學》 第○○○輯, 한국중국어문학회.
- 박수민(2026), sishi-zhiming: 前四史 ‘知名’ 용례 콘코던스, https://github.com/suparklingmin/sishi-zhiming.

## 라이선스

이 저장소의 주석 자료는 CC0 1.0(퍼블릭 도메인 기증)으로 공개합니다. KWIC로 발췌한 원문은 저작권이 소멸된 고전 텍스트이며, 전자 텍스트의 출처는 Chinese Text Project입니다.

## 한계

- 원문은 CTP의 전자 텍스트에 의존했으므로 저본의 글자 오류가 남아 있을 수 있습니다(논문에 인용한 용례는 영인본·點校本과 대조했습니다).
- 裴注 인용 문헌의 성립 시기와 분류 가운데 일부는 추정입니다(`peizhu_source_genre.csv`의 `basis` 열).
- `hhs_subject_period.csv`의 `confidence`가 ‘확인’인 4건 외에는 원문 대조 전의 판정입니다.
- 전승 관계(`parallel_type`)는 현존 문헌 사이에서 확인되는 의존의 하한선일 뿐, 독립성의 증명이 아닙니다.
