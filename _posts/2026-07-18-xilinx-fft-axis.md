---
layout: post
title: "Xilinx FFT IP의 AXI4-Stream 이해하기"
date: 2026-07-18
description: "tvalid, tready, tlast가 실제 스트리밍 FFT에서 어떻게 맞물리는지 흐름 중심으로 설명합니다."
tags: [FPGA, DSP]
---
## AXI4-Stream의 핵심

데이터 전송은 `tvalid`와 `tready`가 동시에 1인 클록에서만 일어난다. 전단은 유효한 데이터를 준비하면 `tvalid`를 올리고, 후단은 받을 수 있을 때 `tready`를 올린다.

## FFT 프레임의 끝

`tlast`는 현재 데이터가 프레임의 마지막 샘플임을 나타낸다. FFT 크기가 2048이라면 일반적으로 2048번째 샘플과 함께 `tlast=1`을 전달한다.

- `s_axis_data_tvalid`: 입력 데이터가 유효함
- `s_axis_data_tready`: FFT가 입력을 받을 준비가 됨
- `m_axis_data_tvalid`: FFT 출력이 유효함
- `m_axis_data_tready`: 후단이 출력을 받을 준비가 됨

## 흔한 실수

`tvalid`만 보고 카운터를 증가시키면 안 된다. 실제 전송 조건인 `tvalid && tready`를 기준으로 샘플 수를 세어야 한다.