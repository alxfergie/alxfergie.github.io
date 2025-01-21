---
date: 2025-01-17-threejs-Renderer
layout: post
title: 노베이스가 말아주는 3D 웹사이트
subtitle: '처음으로 서버에서 3D 도형을 만들고, 세그먼트, 로테이션, 색을 조절할수있어서 뭔가 신기하고 재미있었다.'
description: >-
  R3F를 이용한 Scene, Camera, Renderer 구현하기.
image: 2025-01-17-threejs-renderer/thumb.png
optimized_image: 2025-01-17-threejs-renderer/thumb.png
category: Threejs
tags:
  - Camera
  - Renderer
  - Scene
author: dahyun
paginate: false
---

## 오늘의 기록

오늘은 정말 힘든 날이었다. 코딩 중 여러 에러를 마주하며 React Three Fiber를 활용한 3D 그래픽 애플리케이션을 개발했다. 주요 학습 내용은 다음과 같다

## 학습 내용
- **Scene, Camera, Renderer** 구성 이해
- 소괄호, 중괄호, 대괄호의 차이 학습

## App.jsx 코드
3D 씬을 구성하는 주요 코드로, 다음 요소를 포함한다:

### 주요 컴포넌트
- **Canvas**: Three.js 렌더링 컨텍스트 제공
- **OrbitControls**: 카메라 이동 및 회전 지원
- **AxesHelper**: X, Y, Z 축 시각화
- **GridHelper**: 3D 공간 격자 생성
- **Leva**: 실시간 UI 컨트롤 제공

### 핵심 코드

```jsx
<Canvas
  camera={{
    fov: 75,
    near: 1,
    far: 20,
    position: [3, 3, 0],
  }}
>
  <color attach="background" args={[color.value]} />
  <OrbitControls />
  <axesHelper args={[5]} />
  <gridHelper args={[10, grid.segment]} />
  <ThreeElement />
</Canvas>
```

## ThreeElement.tsx 코드

3D 박스를 정의하고 조작하기 위한 주요 코드로, 다음 기능을 포함합니다:

### 주요 기능
- **useRef**: 특정 요소 참조
- **useFrame**: 프레임마다 호출되어 애니메이션 효과 추가
- **Leva**: 슬라이더로 Y축 회전 동적 조정

### 핵심 코드
```tsx
<mesh
  ref={boxRef}
  rotation={[
    THREE.MathUtils.degToRad(45), // X축 회전
    THREE.MathUtils.degToRad(box.rotation), // Y축 회전 (사용자 조정 가능)
    0 // Z축 회전
  ]}
>
  <boxGeometry /> {/* 박스 형태 */}
  <meshStandardMaterial color="blue" /> {/* 파란색 재질 */}
</mesh>
```

## Scene, Camera, Renderer

### Scene
- 3D 객체와 조명을 포함하는 공간으로, 모든 3D 요소가 추가됩니다.

### Camera
- Scene을 관찰하는 시점을 정의:
  - **fov**: 시야각(Field of View)
  - **near/far**: 렌더링 최소/최대 거리
  - **position**: 초기 위치 `[X, Y, Z]`

### Renderer
- Scene과 Camera를 기반으로 화면에 최종 이미지를 렌더링.

---

## 실행 전 준비
1. **Node.js 버전 설정**

  ```bash
  $ nvm use 18

  $ npm install @react-three/fiber @react-three/drei leva
  ```

## 소괄호, 중괄호, 대괄호 차이 (코딩)

### 소괄호 `()`
- **함수 호출**: `myFunction(arg1, arg2)`
- **표현식 그룹화**: `(a + b) * c`

### 중괄호 `{}`
- **코드 블록 정의**: `if (condition) { /* 실행 코드 */ }`
- **객체 리터럴 선언**: `const obj = { key: value }`

### 대괄호 `[]`
- **배열 선언**: `const arr = [1, 2, 3]`
- **배열 요소 접근**: `arr[0]`
- **동적 키 접근**: `obj[key]`


## 지금까지 완성본

<iframe width="560" height="400" src="https://www.youtube.com/embed/6diwHGZi4C8?si=RSx2sd5WK3pRzwnC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
