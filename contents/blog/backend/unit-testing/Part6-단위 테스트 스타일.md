---
title: Unit Testing 6장 단위 테스트 스타일
thumbnail: ../images/test/unit-testing.png
date: 2023-04-08 14:10:51
category: Back-End
tags: [Test]
draft: false
---

## 📖 6.1 단위 테스트의 세 가지 스타일

___

단위 테스트는 세가지 스타일이 있다.

- 출력 기반 테스트
- 상태 기반 테스트
- 통신 기반 테스트

### 🔖 6.1.1 출력 기반 테스트 정의

SUT에 입력을 넣고 생성되는 출력을 점검하는 방식. 전역 상태나 내부 상태를 변경하지 않는 코드에만 적용되므로 반환 값만 검증하면 된다.

출력 기반 단위 테스트 스타일은 함수형이라고도 한다.

### 🔖 6.1.2 상태 기반 스타일의 정의

작업이 완료된 후 시스템 상태를 확인하는 것이다.

- 상태: SUT나 협력자 중 하나, 데이터베이스나 파일 시스템 등과 같은 프로세스 외부 의존성의 상태

### 🔖 6.1.3 통신 기반 스타일의 정의

목을 사용해 테스트 대상 시스템과 협력자 간의 통신을 검증

> 고전파는 상태 기반 스타일을 선호하고, 런던파는 통신 기반 스타일을 선호한다. 두 분파는 출력 기반 테스트를 사용한다.

## 📖 6.2 단위 테스트 스타일 비교

___

### 🔖 6.2.1 회귀 방지와 피드백 속도 지표로 스타일 비교하기

회귀방지 지표는 특정 스타일에 따라 달라지지 않고, 다음 세 가지 특성으로 결정된다.

- 테스트 중에 실행되는 코드의 양
- 코드 복잡도
- 도메인 유의성

테스트 스타일과 테스트 피드백 속도 사이에는 상관관계가 거의 없다. 테스트가 프로세스 외부 의존성과 떨어져 단위 테스트 영역에 있는 한, 모든 스타일은 테스트 실행 속도가 거의 동일하다.

### 🔖 6.2.2 리팩터링 내성 지표로 스타일 비교하기

리팩터링 내성은 리팩터링 중에 발생하는 거짓 양성 수에 대한 척도다.

- 출력 기반 테스트
  - 테스트가 테스트 대상 메서드에만 결합되므로 거짓 양성 방지가 가장 우수하다.
- 상태 기반 테스트
  - 거짓 양성이 되기 쉽다.
  - 테스트와 제품 코드 간의 결합도가 클수록 유출되는 구현 세부 사항에 테스트가 얽매일 가능성이 커진다.
- 통신 기반 테스트
  - 허위 경보(거짓 양성)에 가장 취약하다.

### 🔖 6.2.3 유지 보수성 지표로 스타일 비교하기

유지 보수성은 다음 두 가지 특성으로 정의한다.

1. 테스트를 이해하기 얼마나 어려운가?
2. 테스트를 실행하기 얼마나 어려운가?

- 출력 기반 테스트
  - 가장 유지 보수하기 용이하다.
  - 프로세스 외부 의존성을 다루지 않기 때문
- 상태 기반 테스트
  - 출력 기반 테스트에 비해 유지 보수가 쉽지 않다.
  - 공간을 더 많이 차지 하기 때문.
- 통신 기반 테스트
  - 가장 유지 보수하기 어렵다.
  - 테스트 대역과 상호 작용 검증을 설정해야 하며, 공간을 많이 차지 하기 때문
  - mock chain이 있을 때, 더욱 유지 보수하기 어려워진다.

### 🔖 6.2.4 스타일 비교하기: 결론

||출력 기반|상태 기반|통신 기반
|:---:|:---:|:---:|:---:
|리팩터링 내성을 지키기 위해 필요한 노력|낮음|중간|중간
|유지비|낮음|중간|높음

출력 기반 테스트를 선호해야 한다.

## 📖 6.3 함수형 아키텍처 이해

___

### 🔖 6.3.1 함수형 프로그래밍이란?

함수형 프로그래밍은 수학적 함수(숨은 입출력이 없는 함수)를 사용한 프로그래밍

#### 수학적 함수

- 수학적 함수의 모든 입출력은 메서드 이름, 인수, 반환 타입으로 구성된 메서드 시그니처에 명시
- 수학적 함수는 호출 횟수에 상관없이 주어진 입력에 대해 동일한 출력 생성
- 입출력을 명시한 수학적 함수는 이에 따르는 테스트가 짧고 간결하며 이해하고 유지 보수하기 쉬우므로 테스트하기가 매우 쉽다.
  - 유지 보수성⬆️, 거짓 양성 빈도⬇️
- 숨은 입출력은 테스트하기 힘들고, 가돋성도 떨어진다.
  - 부작용: 메서드 시그니처에 표시되지 않은 출력
  - 예외: 메서드 시그니처에 설정된 계약을 우회하는 경로를 만듦
  - 내외부 상태에 대한 참조: 메서드 시그니처에 없는 실행흐름에 대한 입력

### 🔖 6.3.2 함수형 아키텍처란?

함수형 프로그래밍의 목표: 부작용을 완전히 제거하는 것이 아니라 비즈니스 로직을 처리하는 코드와 부작용을 일으키는 코드를 분리

- 결정을 내리는 코드
  - 부작용이 필요 없음
  - 함수형 코어(불변 코어)
- 해당 결정에 따라 작용하는 코드
  - 수학적 함수에 의해 이뤄진 모든 결정을 데이터베이스의 변경이나 메시지 버스로 전송된 메시지와 같이 가시적인 부분으로 변환
  - 가변 셸

#### 함수형 코어와 가변 셸의 협력

- 가변 셸은 모든 입력을 수집
- 함수형 코어는 결정을 생성
- 셸은 결정을 부작용으로 변환

#### 캡슐화와 불변성

캡슐화와 같이, 함수형 아키텍처와 불변성은 소프트웨어 프로젝트의 지속적인 성장을 가능하게 하는 것이 목표이다.

> 객체지향 프로그래밍은 작동 부분을 캡슐화해 코드를 이해할 수 있게 한다. 함수형 프로그래밍은 작동 부분을 최소화해 코드를 이해할 수 있게 한다.

### 🔖 6.3.3 함수형 아키텍처와 육각형 아키텍처 비교

#### 공통점

- **관심사 분리** 기반
- 의존성 간의 단방향 흐름
  - 육각형 아키텍처: 도메인 계층 내 클래스는 서로에게만 의존해야 함.
  - 함수형 아키텍처: 불변 코어는 가변 셸에 의존하지 않음.

#### 차이점

- 부작용에 대한 처리
  - 함수형 아키텍처: 모든 부작용을 불변 코어에서 비즈니스 연산 가장자리로 밀어내어 가변 셸이 처리
  - 육각형 아키텍처: 도메인 계층에 제한하는 한, 부작용은 문제없음.

> 함수형 아키텍처는 육각형 아키텍처의 하위 집합이다.

## 📖 6.4 함수형 아키텍처와 출력 기반 테스트로의 전환

___

이 절에서는 샘플 코드를 함수형 아키텍처로 리팩터링 하는 것을 보여준다. 두 가지 리팩터링 단계를 보여준다.

1. 프로세스 외부 의존성에서 Mock으로 변경
2. Mock에서 함수형 아키텍처로 변경

### 🔖 6.4.1 감사 시스템 소개

조직의 모든 방문자를 추적하는 감사 시스템이 샘플이다.

아래 코드는 초기 버전이다.

```java
@RequiredArgsConstructor
public class AuditManager {

    private final int maxEntriesPerFile;
    private final String directoryName;

    public void addRecord(String visitorName, Date timeOfVisit) throws IOException {
        String[] filePaths = new File(directoryName).list();
        Arrays.sort(Objects.requireNonNull(filePaths), Comparator.comparingInt(this::extractIndex));

        String newRecord = visitorName + ";" + timeOfVisit.toString();

        if (filePaths.length == 0) {
            String newFile = Paths.get(directoryName, "audit_1.txt").toString();
            Files.write(Paths.get(newFile), Collections.singletonList(newRecord));
            return;
        }

        String currentFilePath = filePaths[filePaths.length - 1];
        Path path = Paths.get(currentFilePath);
        List<String> lines = Files.readAllLines(path);

        if (lines.size() < maxEntriesPerFile) {
            lines.add(newRecord);
            Files.write(path, lines);
            return;
        }

        String newFile = Paths.get(directoryName, "audit_" + extractIndex(currentFilePath) + 1 + ".txt").toString();
        Files.write(Paths.get(newFile), Collections.singletonList(newRecord));
    }

    private int extractIndex(String filePath) {
        String fileName = new File(filePath).getName();
        return Integer.parseInt(fileName.substring(6, fileName.lastIndexOf(".")));
    }
}
```

`AuditorManager` class는 파일 시스템과 밀접하게 연결돼 있어 그대로 테스트하기가 어렵다.

### 🔖 6.4.2 테스트를 파일 시스템에서 분리하기 위한 목 사용

파일의 모든 연산을 인터페이스로 도출한 후 AuditManager 클래스에 IFileSystemImpl 구현 클래스를 주입 받아 사용할 수 있다.

```java
public interface FileSystemInterface {

    String[] getFiles(String directoryName);

    void writeAllText(String filePath, String content);

    List<String> readAllLines(String filePath);
}
```

```java
@RequiredArgsConstructor
public class AuditManager {

    private final int maxEntriesPerFile;
    private final String directoryName;
    private final FileSystemImpl fileSystem;

    public void addRecord(String visitorName, Date timeOfVisit) throws IOException {
        String[] filePaths = fileSystem.getFiles(directoryName);
        Arrays.sort(filePaths, Comparator.comparingInt(this::extractIndex));

        String newRecord = visitorName + ";" + timeOfVisit.toString();

        if (filePaths.length == 0) {
            String newFile = Paths.get(directoryName, "audit_1.txt").toString();
            fileSystem.writeAllText(newFile, newRecord);
            return;
        }

        String currentFilePath = filePaths[filePaths.length - 1];
        List<String> lines = fileSystem.readAllLines(currentFilePath);

        if (lines.size() < maxEntriesPerFile) {
            lines.add(newRecord);
            String newContent = String.join("\r\n", lines);
            fileSystem.writeAllText(currentFilePath, newContent);
            return;
        }
        String newName = "audit_" + extractIndex(currentFilePath) + 1 + ".txt";
        String newFile = Paths.get(directoryName, newName).toString();
        fileSystem.writeAllText(newFile, newRecord);
    }

    private int extractIndex(String filePath) {
        String fileName = Paths.get(filePath).getFileName().toString();
        return Integer.parseInt(fileName.substring(6, fileName.lastIndexOf(".")));
    }
}
```

이제 공유 의존성이 사라지고 테스트를 서로 독립적으로 실행할 수 있다. Mock을 사용한다면 더 이상 파일 시스템에 접근하지 않으므로 더 빨리 실행된다. 초기 버전보다 더욱 개선이 된 것이다.

### 🔖 6.4.3 함수형 아키텍처로 리팩터링하기

`AuditorManager` 는 함수형 코어에 해당된다.

```java
@RequiredArgsConstructor
public class AuditManager {

    private final int maxEntriesPerFile;

    public FileUpdate addRecord(FileContent[] files, String visitorName, LocalDateTime timeOfVisit) {
        Arrays.sort(files);
        String newRecord = visitorName + ";" + timeOfVisit;

        if (files.length == 0) {
            return new FileUpdate("audit_1.txt", newRecord);
        }

        int currentFileIndex = files.length - 1;
        FileContent currentFile = files[currentFileIndex];
        List<String> lines = Arrays.asList(currentFile.getLines());

        if (lines.size() < maxEntriesPerFile) {
            lines.add(newRecord);
            return new FileUpdate(currentFile.getFileName(), String.join("\r\n", lines));
        }

        return new FileUpdate("audit_" + currentFileIndex + 1 + ".txt", newRecord);
    }
}
```

```java
@Getter
public record FileContent(String fileName, String[] lines) {
}
```

```java
@Getter
public record FileUpdate(String fileName, String newContent) {
}
```

`Persister`는 가변 셸 역할을 한다.

```java
public class Persister {

    public FileContent[] readDirectory(String directoryName) {
        return Arrays.stream(Objects.requireNonNull(new File(directoryName).listFiles()))
                .map(file -> {
                    try {
                        return new FileContent(file.getName(), Files.readAllLines(file.toPath()).toArray(String[]::new));
                    } catch (IOException e) {
                        throw new RuntimeException(e);
                    }
                })
                .toArray(FileContent[]::new);
    }

    public void applyUpdate(String directoryName, FileUpdate update) {
        String filePath = Paths.get(directoryName, update.getFileName()).toString();
        try {
            Files.write(Paths.get(filePath), update.getNewContent().getBytes());
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

```java
public class ApplicationService {

    private final String directoryName;
    private final AuditManager auditManager;
    private final Persister persister;

    public ApplicationService(String directoryName, int maxEntriesPerFile) {
        this.directoryName = directoryName;
        this.auditManager = new AuditManager(maxEntriesPerFile);
        this.persister = new Persister();
    }

    public void addRecord(String visitorName, LocalDateTime timeOfVisit) {
        FileContent[] files = persister.readDirectory(directoryName);
        FileUpdate update = auditManager.addRecord(files, visitorName, timeOfVisit);
        persister.applyUpdate(directoryName, update);
    }
}
```

위와 같이 함수형 코어와 가변 셸을 붙이면 진입점이 제공된다. 모든 테스트는 작업 디렉터리의 가상 상태를 제공하고 `AuditorManager`가 내린 결정을 검증하는 것으로 단축됐다.

||초기 버전|Mock 사용|출력 기반
|:---:|:---:|:---:|:---:
|회귀 방지|좋음|좋음|좋음
|리팩터링 내성|좋음|좋음|좋음
|빠른 피드백|나쁨|좋음|좋음
|유지 보수성|나쁨|중간|좋음

### 🔖 6.4.4 예상되는 추가 개발

함수형 아키텍처는 추가 요구사항에 대해 쉽게 개발과 대응을 할 수 있다.

## 📖 6.5 함수형 아키텍처의 단점 이해하기

___

함수형 아키텍처라고해도, 코드베이스가 커지고 성능에 영향을 미치면서 유지 보수성의 이점이 상쇄된다.

### 🔖 6.5.1 함수형 아키텍처 적용 가능성

숨은 입력이 생기면 수학적 함수가 될 수 없으며, 출력 기반 테스트를 적용할 수 없다.

이에 대한 해결은 성능 저하 혹은 아키텍처가 망가지는 결과를 가져온다.

### 🔖 6.5.2 성능 단점

함수형 아키텍처와 전통적인 아키텍처 사이의 선택은 성능과 코드 유지 보수성 간의 절충이다. 성능 영향이 그다지 눈에 띄지 않는 일부 시스템에서는 함수형 아키텍처를 사용해 유지 보수성을 향상시키는 편이 낫다.

### 🔖 6.5.3 코드베이스 크기 증가

함수형 아키텍처는 코드 복잡도가 낮아지고 유지 보수성이 증가하지만, 초기에 코딩이 더 필요하다.
또한, 함수형 방식에서 순수성에 많은 비용이 든다면 순수성을 따르지 말라.
