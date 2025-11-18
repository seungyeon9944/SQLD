### 2. TCL

Transaction Control Language, **트랜잭션** (논리적 업무 단위) 제어

- 원자성 (Automicity)
- 일관성 (Consistency)
- 고립성 (Isolation)
- 지속성 (Durability)

### `COMMIT`

최종적으로 데이터 파일에 기록, 트랜잭션 완료

### `ROLLBACK`

INSERT, DELETE, UPDATE 후 변경된 내용 취소

### `SAVEPOINT`

**일부만 되돌릴 수 있게** 하는 기능

SAVEPOINT A ~ ROLLBACK TO A는 없는 셈 !