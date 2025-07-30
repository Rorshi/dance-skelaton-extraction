## 🗂️ 언어 선택 | Language
- 🇰🇷 [한국어로 보기](#한국어-설명서)
- 🇺🇸 [Read in English](#english-documentation)

## 주요 버전 관리:
- 1.0.0v:
> 고정밀 다중 인원 3D 자세 추적 및 보정 기본 동작 확인
> 

<br>
---

## 한국어 설명서
# 🧍‍♀️ Multi-Person 3D Pose Tracking and Correction Pipeline
> 고정밀 다중 인원 3D 자세 추적 및 보정 파이프라인

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
<br>

## 📝 1. 프로젝트 개요
이 프로젝트는 댄스 영상처럼 여러 사람이 복잡하게 움직이는 상황에서도 각 인물의 3D 스켈레톤(관절) 정보를 정확하게, 끊김 없이, 일관성 있게 ID를 추적하는 것을 목표로 합니다.
이를 위해 다음과 같은 하이브리드 처리 파이프라인을 설계하였습니다.
- **STAGE 1**: YOLO + MediaPipe + Kalman Filter를 활용한 실시간 추적
- **STAGE 2**: 후처리로 ID 오류 및 떨림 제거를 통한 신뢰도 향상
- **STAGE 3**: 최종 결과를 시각적으로 렌더링
결과적으로, 분석에 바로 활용 가능한 고품질의 analysis-ready 데이터를 생성합니다.

## 🧰 2. 주요 기술 스택
| 기능          | 기술                                      |
| ----------- | --------------------------------------- |
| **객체 탐지**   | YOLOv8                                  |
| **자세 추정**   | MediaPipe Pose                          |
| **데이터 분석**  | NumPy, Pandas                           |
| **추적 & 매칭** | Kalman Filter (3D), Hungarian Algorithm |
| **후처리 보정**  | NetworkX, SciPy (Savitzky-Golay)        |
| **시각화**     | OpenCV                                  |

## 🔁 3. 전체 파이프라인 요약
graph TD
    A[비디오 입력] --> B[YOLOv8로 인물 탐지]
    B --> C[슬롯 기반 트래킹 및 Kalman 보정]
    C --> D[MediaPipe로 3D 관절 추출]
    D --> E[스켈레톤 위치 스무딩]
    E --> F[CSV 및 영상 출력]

    F --> G[후처리: ID 스위치 보정 (NetworkX)]
    G --> H[전역 스무딩 (Savitzky-Golay)]
    H --> I[운동학 검증 및 오류 점수]
    I --> J[최종 CSV + 시각화 영상 출력]