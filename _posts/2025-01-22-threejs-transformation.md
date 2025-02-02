---
date: 2025-01-22-threejs-transformation
layout: post
title: 노베이스가 말아주는 3D Transformation
subtitle: '그룹화된 객체의 제어와 로컬-월드 좌표계의 차이를 중심으로 3D 공간에서의 구조적 활용을 심화'
description: >-
  R3F를 이용한 position, scale, rotation 구현하기.
image: 2025-01-22-threejs-transformation/thumbnail.png
optimized_image: 2025-01-22-threejs-transformation/thumbnail.png
category: Threejs
tags:
  - Position
  - Scale
  - Rotation
author: dahyun
paginate: false
---
# 요약
이번 강의에서는 **Group**을 활용하여 다수의 Mesh를 제어하고, 애니메이션과 조명 효과를 추가하는 방법을 배웠습니다.  
저번 강의에서는 단일 Mesh와 기본적인 애니메이션에 초점이 맞춰졌지만, 이번 강의는 그룹화된 객체의 제어와 로컬-월드 좌표계의 차이를 중심으로 3D 공간에서의 구조적 활용을 심화했습니다.

# 주요 개념 설명

## 1. Position, Scale, Rotation

### **Position**
- 객체의 위치를 정의합니다.
- `[x, y, z]` 값으로 객체의 위치를 설정합니다.
  - **World Position**: 전체 3D 공간에서의 절대 위치.
  - **Local Position**: 부모 객체(Group 등) 기준으로 정의된 상대 위치.

---

### **Scale**
- 객체의 크기를 정의합니다.
- `[x, y, z]` 값으로 객체의 크기를 조정합니다.
  - 예: `scale={[1, 2, 1]}`은 y축 크기를 2배로 확대합니다.

---

### **Rotation**
- 객체의 회전 각도를 정의합니다.
- `[x, y, z]` 값으로 각 축을 기준으로 회전하며, **라디안 단위**를 사용합니다.
  - `THREE.MathUtils.degToRad()`를 통해 각도를 라디안으로 변환할 수 있습니다.

---

## 2. Object3D의 계층 구조

### **Scene**
- 3D 공간의 최상위 컨테이너로, 모든 객체와 조명을 포함합니다.

---

### **Group**
- 여러 Mesh 객체를 묶는 컨테이너로, 로컬 좌표계를 공유합니다.

---

### **Mesh**
- 실제 3D 객체로, **Geometry(형태)**와 **Material(재질)**의 조합입니다.

#### **Geometry**
- 객체의 형태를 정의합니다. (예: 박스, 구, 평면 등)

#### **Material**
- 객체의 표면 특성(색상, 반사율 등)을 정의합니다.

---

### **Mesh = Geometry + Material**
- `boxGeometry`와 같은 Geometry는 객체의 형태를 정의합니다.
- `meshStandardMaterial`과 같은 Material은 객체의 색상과 빛 반사 특성을 설정합니다.


# 주요 코드 설명: `ThreeElement.tsx`

이번 강의에서는 **Group**을 중심으로 다수의 **Mesh**를 생성하고, 애니메이션을 추가하는 방법을 다뤘습니다.

---

## 1. 주요 코드 구성

### **1) Group 생성**
- `Group`은 여러 `Mesh` 객체를 묶는 컨테이너입니다.
- 로컬 좌표계를 공유하며, 그룹 단위로 위치, 크기, 회전을 조정할 수 있습니다.

```jsx
// ThreeElement.tsx

return(
    <>
        <directionalLight position={[5,5,5]}/>
        <group
            ref={groupRef}
            position={[0,0,3]}
            rotation={[
                THREE.MathUtils.degToRad(0), 
                THREE.MathUtils.degToRad(0),
                THREE.MathUtils.degToRad(0)
            ]}
        >
            <axesHelper args={[3]}/>
            <mesh 
                ref={boxRef}
                position={[0,0,2]}
                scale={[1,1,1]}
                rotation={[
                    THREE.MathUtils.degToRad(0), 
                    THREE.MathUtils.degToRad(0),
                    THREE.MathUtils.degToRad(0)
                ]}
            >
                <axesHelper args={[3]}/>
                <boxGeometry />
                <meshStandardMaterial color="red"/> 
            </mesh>
            <mesh 
                ref={boxRef}
                position={[2,0,0]}
                scale={[1,1,1]}
                rotation={[
                    THREE.MathUtils.degToRad(0), 
                    THREE.MathUtils.degToRad(0),
                    THREE.MathUtils.degToRad(0)
                ]}
            >
                <axesHelper args={[3]}/>
                <boxGeometry />
                <meshStandardMaterial color="blue"/> 
            </mesh>
            <mesh 
                ref={boxRef}
                position={[0,2,0]}
                scale={[1,1,1]}
                rotation={[
                    THREE.MathUtils.degToRad(0), 
                    THREE.MathUtils.degToRad(0),
                    THREE.MathUtils.degToRad(0)
                ]}
            >
                <axesHelper args={[3]}/>
                <boxGeometry />
                <meshStandardMaterial color="green"/> 
            </mesh>
        </group>
)
```
#### Group
- `position={[0, 0, 3]}`은 그룹의 위치를 설정.

#### Mesh
- **빨간 박스**는 `(0, 0, 0)`에 위치.
- **파란 박스**는 `(2, 0, 0)`에 위치.
- **초록 박스**는 `(0, 2, 0)`에 위치.

#### 애니메이션 추가
- 그룹 전체를 **x축 기준으로 회전**.
- **useFrame**: 매 프레임마다 호출되어 애니메이션 효과를 추가합니다.
- 그룹 전체가 함께 회전하며, 각 Mesh는 상대적인 위치를 유지합니다.

```jsx
const { size, gl, scene, camera } = useThree();
const groupRef = useRef<THREE.Mesh>(null);

useFrame((state, delta) => {
    scene.rotation.x += 0.01; // Scene
    groupRef.current.rotation.x += delta; // Group 
})

scene.rotation.x = THREE.MathUtils.degToRad(45) // Scene
```

# App.tsx 코드

## 이번 강의에서의 설정:

```jsx
//App.tsx

function App() {

  return(
    <>
        <Canvas
          camera=
        >
          <color attach="background" args={["white"]}/>
          <OrbitControls/>
          <axesHelper args={[6]}/>  // ← 얘
          <gridHelper args={[10, 10]}/>
          <ThreeElement/>
        </Canvas>
    </>
  )
}

export default App
```

- **DirectionalLight 추가**: 3D 장면에 조명을 설정.
- **GridHelper와 AxesHelper**: 위치를 시각화.
- **DirectionalLight**: 빛의 방향을 설정하여 명암 효과를 추가.
- **ThreeElement**: 다수의 Mesh를 포함하는 Group과 애니메이션 적용.



# 저번 강의와의 차이점
## 1) Object3D 계층 구조
- **저번 강의**: 단일 Mesh만 다룸. Scene에서 바로 Mesh를 추가.
- **이번 강의**: Group을 활용하여 다수의 Mesh를 묶어 로컬 좌표계로 제어.

## 2) 다수 객체 생성
- **저번 강의**: 하나의 Mesh만 생성.
- **이번 강의**: 빨간색, 파란색, 초록색 박스를 포함한 3개의 Mesh를 생성.

## 3) 애니메이션
- **저번 강의**: 단일 Mesh의 회전 애니메이션 적용.
  ```tsx
  useFrame((state, delta) => { 
    boxRef.current.rotation.y += delta; 
  });
  ```
- **이번 강의**: Group전체에 회전 애니메이션 적용.
   ```tsx
  useFrame((state, delta) => { 
  groupRef.current.rotation.x += delta; 
  });
  ```
## 4) 조명효과
- **저번 강의**: 조명이 없었으며, Mesh의 기본 색상만 사용.
- **이번 강의**: DirectionalLight를 추가하여 빛과 그림자를 표현.

# 실행방법 정리

1. Node.js설정:
```bash
$ nvm use 18
```

2. 필요한 패키지 설치
```bash
$ npm install @react-three/fiber @react-three/drei 
```

3. 개발 서버 실행
```bash
$ npm run dev 
```

# 느낀점
이번 강의는 3D 공간을 구조적으로 이해하고 활용하는 데 많은 도움이 되었다.  
특히 **Group**을 활용해 객체를 묶고, **로컬 좌표계와 월드 좌표계**의 차이를 명확히 이해할 수 있었다.

조명을 추가하면서 단순한 박스들이 더 현실감 있게 보였고, 애니메이션을 통해 장면이 생동감 있게 표현되는 점이 흥미로웠다.

**React Three Fiber**를 활용해 복잡한 3D 로직을 간결하게 구현할 수 있다는 점에서 작업이 더 직관적이고 재미있게 느껴졌다.  
앞으로 이 개념들을 활용해 더 창의적인 3D 장면을 만들어보고 싶다.