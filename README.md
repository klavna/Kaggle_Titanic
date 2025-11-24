# MoRel

이 저장소는 MoRel의 학습, 렌더링, 평가를 위한 공식 코드입니다.  
아래는 Train / Render / Metric 실행 방법만 정리합니다.

---

## Running

### 1. Training

```bash
python train.py \
  -s data/<dataset>/<scene_path> \
  --port <port_number> \
  --expname "<experiment_name>/<scene_name>" \
  --configs arguments/<dataset>/config_<cfg_id>.py \
  --dataset_configs arguments/<dataset>/dataset_config/<scene_name>.py \
  [--fine_tuning]
```

인자 설명
- `-s` (`--source_path`): 학습할 씬(시퀀스) 경로.
- `--expname`: 실험 이름 및 출력 폴더 식별자. 결과는 `output/<expname>` 아래에 저장됩니다.
- `--configs`: 데이터셋 공통 설정 파일. 예: `arguments/<dataset>/config_1.0.py`.
- `--dataset_configs`: 씬별 프레임 구간/길이 등 데이터셋 구성 설정 파일.
- `--fine_tuning`: 파인튜닝 모드 활성화 플래그(필요 시에만 추가).
- `--port`: 뷰어/서버 포트 번호(뷰어 사용 시 필요).

---

### 2. Rendering

```bash
python render.py \
  --model_path "output/<experiment_name>/<scene_name>" \
  --configs arguments/<dataset>/config_<cfg_id>.py \
  --render_frame_end <frame_end> \
  --skip_video
```

옵션 설명
- `--model_path`: 학습이 완료된 모델 경로. 예: `output/<experiment_name>/<scene_name>`.
- `--configs`: 학습에서 사용한 설정 파일과 동일한 config를 지정합니다.
- `--render_frame_end`: 렌더링할 마지막 프레임 인덱스(시퀀스 길이 제어).
- `--skip_train`: train 뷰 렌더링을 생략합니다.
- `--skip_test`: test 뷰 렌더링을 생략합니다.
- `--skip_video`: trajectory/interpolation 비디오 렌더링을 생략합니다.
- `--canonical_frame_render`: canonical frame만 별도로 렌더링합니다.
- `--forward_only`: forward 방향 결과만 렌더링합니다.
- `--backward_only`: backward 방향 결과만 렌더링합니다.
- `--streaming_video`: demo/스트리밍 목적의 비디오 렌더링을 수행합니다.

---

### 3. Evaluation

```bash
python metrics.py \
  --model_path "output/<experiment_name>/<scene_name>"
```

인자 설명
- `--model_path`: 평가할 모델 경로.

---

### 4. Collecting Metrics by Dataset

```bash
python collect_metric.py \
  --output_path "output/<experiment_name>" \
  --dataset <dataset_name>
```

인자 설명
- `--output_path`: 데이터셋 단위로 결과를 모을 실험 루트 경로.
- `--dataset`: 집계 대상 데이터셋 이름(예: `selfcap`, `dycheck`).

설명
- `metrics.py`는 씬 단위 PSNR/SSIM/LPIPS 등 기본 지표를 계산합니다.
- `collect_metric.py`는 데이터셋 내 여러 씬 결과를 모아 텍스트 파일로 요약합니다.
