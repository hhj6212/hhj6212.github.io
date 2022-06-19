---
layout: post
title: AWS Database (RDS & DynamoDB)
comments: true
categories: [tech,programming,cloud,aws]
---

AWS Database service 에 대해 공부한 내용을 정리한 것입니다.

- Relational Database Service (RDS)
- Read Replica

---
### Relational Database Service (RDS)

#### RDS 란?
AWS 에서 제공하는 관계형 DB 서비스.
다음 engine 들을 사용해서 만들 수 있다.
- Microsoft SQL Server
- Oracle
- MySQL
- PostgreSQL
- MariaDB
- Amazon Aurora

AWS 에서 RDS 를 검토하고 생성하게 되면 몇분 안에 RDS instance 가 생성된다.
이 때, 위의 6개 엔진 중 하나로 돌아가는 EC2가 만들어지게 되는데, 이 EC2 OS 에 접근할 수는 없다.
Automated backups: 설정 주기에 따라 RDS 를 자동으로 백업할 수 있다.
Multi-AZ: 여러 availability zone 에 만들 수 있는데, 하나의 AZ 에 만들어진 DB 를 다른 AZ 에 그대로 복사하는 것이다.
이 때, 첫 번째 AZ 에 만들어진 DB 가 fail 되는 경우, AWS 에서는 automatic fail over 라는 기능으로 해당 DB 를 참조하던 모든 EC2 가 두 번째 AZ 의 DB 를 참조할 수 있도록 해준다.

| ![-]({{"/assets/220619/image2.png"| relative_url}}) | 
|:--:| 
| *특정 AZ 에 만들어진 primary DB 가 망가진 경우 다른 AZ 에 만들어진 secondary DB 를 참조할 수 있도록 하는 Automatic fail over 기능.* |

#### RDS database 는 언제 사용하는 걸까?
RDS 는 **online transaction processing (OLTP)** workload 에 주로 사용된다.
이는 **online analytical processing (OLAP)** 에 주로 사용되는 **RedShift** 와 비교할 수 있다. 

![-]({{"/assets/220619/image1.png"| relative_url}})

#### 다음은 RDS 를 생성하는 예시 과정이다.

| ![-]({{"/assets/220619/image3.png"| relative_url}}) | 
|:--:| 
| *RDS console -> Create RDS 로 생성한다. 이 아래에서 Engine, Template, User/PW, instance class (EC2 와 유사함), storage, AZ, Connectivity (VPC), Authentication 등을 선택할 수 있다.* |

| ![-]({{"/assets/220619/image4.png"| relative_url}}) | 
|:--:| 
| *맨 아래쪽에서 이 RDS 의 monthly cost 를 확인해볼 수 있다.* |

| ![-]({{"/assets/220619/image5.png"| relative_url}}) | 
|:--:| 
| *생성된 RDS 의 summary page. 여기서 접속에 필요한 Endpoint 및 Port 를 확인할 수 있다.* |

---

### Read Replica

#### Read Replica 란?
Read-only copy of a primary database.
Database 의 성능 (Performance) 를 높이기 위해 사용하는 기능이다.
Primary database 는 App server 에 의해 Read/Write 가 가능하다.
Read Replica 는 이 primary DB 의 복사본이며, 해당 DB 는 '사용자' 들이 Read 만 수행하게 된다.
따라서 사용량이 많은 App 및 Database 에 유용한 기능이다.

Read replica 는 다음과 같은 특징을 가진다.
- Read replica 는 같은 AZ 에 만들 수도 있고, 다른 AZ 에도 만들 수 있다.
- 각 read replica 는 개별 DNS endpoint 를 가지고 있다.
- Read replica 를 primary DB 와 분리시켜서 새로운 primary DB 로 만들 수도 있다. (Promote) 이 때, 기존 primary DB 와의 연결은 해제된다.
- Read replica 를 생성하기 위해서는 Automatic Backup 이 활성화 되어있어야 한다.
- 각 DB instance 에 대해 최대 5개의 read replica 를 생성할 수 있다.

![-]({{"/assets/220619/image6.png"| relative_url}})

#### 다음은 특정 RDS 의 Read Replica 를 생성하는 과정의 예시이다.

| ![-]({{"/assets/220619/image7.png"| relative_url}}) |
|:--:|
| *만들어진 RDS 를 선택한다.* |

| ![-]({{"/assets/220619/image8.png"| relative_url}}) |
|:--:|
| *오른쪽 위 'Actions' 에서 Create read replica 를 선택한다.* |

| ![-]({{"/assets/220619/image9.png"| relative_url}}) |
|:--:|
| *그러면 새로 RDS 를 만들 때와 비슷하게 network, connectivity, authentication 등을 선택하게 된다.* |

| ![-]({{"/assets/220619/image10.png"| relative_url}}) |
|:--:|
| *목록에서 primary 밑에 replica 가 생성된 것을 알 수 있다.* |

| ![-]({{"/assets/220619/image11.png"| relative_url}}) |
|:--:|
| *만들어진 replica 를 선택하고 'Actions' -> promote 를 선택해서 해당 replica 를 primary DB 로 만들 수 있다.* |

#### Multi-AZ 와 Read-replica 의 차이점
'시스템에 문제가 생겼을 때 대비하는 기능' vs '성능을 높이는 기능' 의 차이로 생각하면 된다.

![-]({{"/assets/220619/image12.png"| relative_url}})

---

네, 오늘은 AWS Database service 에 대해 공부한 내용을 정리해보았습니다.
그럼 다음 시간에 만나요!