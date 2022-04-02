# MySQL Replication(Master & Slave)
#1. 읽기와 수정 요청을 분산 시키기 위해 Replication을 사용한다.  
master와 slave를 사용하여 master의 경우 트렌젝션 격리 수준을 높게 잡아 CUD를 전담하고 slave의 경우는 R을 담당하여 수정과 조회의 요청을 분산처리할 수 있다.(속도 향상 및 일관성 유지)  

