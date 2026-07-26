---
title: "Building a Brain FEM Pipeline for Dementia Research"
date: 2026-07-27 22:00:00 +0900
categories: [Research, FEM]
tags: [Dementia, FEM, SimNIBS, MRI, OASIS, ECT, ECVT, Python]
toc: true
math: true
---

# Building a Brain FEM Pipeline for Dementia Research

## Overview

최근 연구실에서는 **FEM(Finite Element Method)을 이용한 치매 진단 연구**를 진행하고 있다.

본 연구의 목표는 MRI 영상으로부터 환자 맞춤형 뇌 모델을 생성하고, 이를 이용하여 뇌 내부의 전기적 특성을 계산한 후 향후 **ECT(Electrical Capacitance Tomography)** 및 **ECVT(Electrical Capacitance Volume Tomography)** 기반 치매 조기진단 시스템 개발에 활용하는 것이다.

기존 MRI 기반 진단은 비용이 높고 실시간 모니터링이 어렵다. 따라서 본 연구에서는 **뇌 유전율(Permittivity)** 과 **전도도(Conductivity)** 의 변화를 계산적으로 분석하여 비침습적인 치매 진단 기술 개발을 목표로 한다.

---

# Research Objective

본 연구에서 수행하는 목표는 다음과 같다.

- MRI 기반 환자 맞춤형 Brain FEM 모델 구축
- Brain Mesh 생성
- 전기장(Electric Field) 계산
- 뇌 유전율 및 전도도 분석
- FEM 기반 ECT/ECVT 영상 재구성
- 치매 진단을 위한 계산 모델 구축

---

# Overall Pipeline

```text
MRI (OASIS Dataset)
        │
        ▼
3D Slicer
(Segmentation)
        │
        ▼
Gmsh
(Mesh Generation)
        │
        ▼
SimNIBS
(FEM Simulation)
        │
        ▼
Electric Field Analysis
        │
        ▼
EIT / ECT Reconstruction
        │
        ▼
Visualization
```

---

# Dataset

본 연구에서는 **OASIS(Open Access Series of Imaging Studies)** MRI 데이터를 사용하였다.

MRI 영상은 Brain Segmentation을 수행한 후 FEM 계산이 가능한 3차원 모델로 변환하였다.

---

# Software Stack

| Software | Purpose |
|-----------|-------------------------|
| OASIS | MRI Dataset |
| 3D Slicer | Brain Segmentation |
| Gmsh | Tetrahedral Mesh |
| SimNIBS | FEM Simulation |
| Python | Pipeline Automation |
| NumPy | Numerical Computing |
| SciPy | Sparse Matrix Solver |
| PyAMG | AMG Solver |
| Matplotlib | Visualization |

---

# MRI Segmentation

먼저 MRI 데이터를 3D Slicer에서 분할하였다.

분할된 조직은 STL 형태로 저장한 뒤 FEM 계산을 위한 Geometry 모델로 사용하였다.

---

# Mesh Generation

분할된 STL 모델은 Gmsh를 이용하여 Tetrahedral Mesh로 변환하였다.

Mesh 품질은 FEM 계산 결과에 직접적인 영향을 주므로 Element Quality를 확인하며 생성하였다.

---

# FEM Simulation

생성된 Mesh를 SimNIBS에서 불러와 FEM 계산을 수행하였다.

계산된 결과는

- Electric Potential
- Electric Field
- Conductivity
- Permittivity

분석에 사용된다.

---

# Conductivity Simulation

치매가 진행될 경우 뇌 조직의 전기적 특성이 변화한다고 알려져 있다.

이를 확인하기 위해 정상 뇌와 치매 뇌의 Conductivity를 비교하는 간단한 모델을 구현하였다.

![Conductivity Comparison](/assets/eit_conductivity_comparison.png)

*Figure 1. Comparison of conductivity distributions between the normal brain, dementia brain, and reconstructed conductivity.*

좌측은 정상 뇌의 Conductivity를 나타내며,

가운데는 치매 환경을 가정하여 Conductivity를 감소시킨 결과이다.

우측은 EIT 역문제를 통해 재구성한 Conductivity 영상이다.

이를 통해 전기적 특성의 변화가 계산적으로 복원될 수 있음을 확인하였다.

---

# Challenges

프로젝트를 진행하면서 여러 문제를 경험하였다.

## Mesh Compatibility

프로그램마다 Mesh 형식이 달라 여러 차례 변환 작업이 필요하였다.

특히 SimNIBS에서 사용할 수 있는 Mesh 형식으로 변환하는 과정이 가장 어려웠다.

---

## Large-scale FEM

Brain Mesh는 수십만 개의 Node와 수백만 개의 Element를 포함한다.

기존 Solver는 계산 속도와 메모리 사용량이 매우 컸기 때문에 Sparse Matrix 기반 Solver와 PyAMG를 사용하여 계산 성능을 향상시켰다.

---

# Future Work

앞으로는

- Brain Permittivity 분석
- ECT/ECVT Reconstruction
- Inverse Problem
- Sensitivity Matrix 계산
- 환자별 FEM 모델 구축
- 치매 진단 알고리즘 개발

을 수행할 예정이다.

---

# Conclusion

본 연구에서는 MRI 기반 Brain FEM Pipeline을 구축하고 SimNIBS를 이용한 전기장 시뮬레이션 환경을 구현하였다.

또한 Conductivity 변화에 따른 EIT Reconstruction 결과를 확인하여 향후 치매 조기진단 시스템 개발 가능성을 확인하였다.

향후에는 실제 환자 데이터를 이용한 FEM 기반 치매 진단 모델 개발을 목표로 연구를 계속 진행할 예정이다.
