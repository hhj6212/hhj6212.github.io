---
layout: post
title: AWS CloudFormation
comments: true
categories: [tech,programming]
---

AWS CloudFormation 에 대해 공부하고 정리한 내용입니다.

---

### 용어 정리
**Templates:**
- yaml or json 으로 되어있는 텍스트 파일.
- CluodFormation 의 input 으로 사용됨.
- Infrastructure 의 endstate 가 정리되어 있음.

**Stack:**
- CloudFormation 이 template 를 실행하면 만들어지는 것.
- template 에 있는 자원을 갱신하려면 해당 stack 을 갱신해야 함.
- 관련된 자원들의 집합을 하나의 단위로써 "stack" 이라고 부른다.
- EC2, Elastic Load Balancing, VPC, S3, ...

**Change set:**
- stack 을 갱신할 때 changet set 을 만들 수 있다.
- change set 은 해당 change 가 현재 실행되고 있는 자원에 어떤 영향을 주는지 볼 수 있게 해준다.
- live system 에서 매우 중요함.

---

**기존에 cloud server 를 사용할 때의 문제점:**
- configuring / managing servers

**이를 Custom script 로 관리하고자 했을 때의 문제점:**
- Error prone
- Point solution
- Difficult to change and maintain
- Rarely made truly reusable

---
### CloudFormation
Infrastructure as Code tool for AWS
Cloud formation 은 AWS 의 무료 서비스이므로, 추가 요금이 발생하지 않는다.

### Cloud formation 의 기능
1. **Create**: template 에 기반해 AWS infrastructure 를 생성
2. **Update**: AWS infrastructure 에 필요한 기능을 업데이트
3. **Delete**: 생성한 AWS infrastructure 를 쉽게 제거

![-]({{"/assets/220514/image1.png"| relative_url}})

### CloudFormation 을 사용하면 다음 작업들이 가능하다:
- template 를 이용해 AWS infrastructure 를 구상 & 설치
- provision & configuration 자동화
- dependency 관리
- infrastructure 의 변화를 쉽게 조절 & 트래킹
- 깔끔한 Rollback or Delete

### 사용 예시

| ![-]({{"/assets/220514/image2.png"| relative_url}}) | 
|:--:| 
| *위와 같은 template 파일을 작성* |
|:--:| 
| ![-]({{"/assets/220514/image3.png"| relative_url}}) | 
|:--:| 
| *AWS CloudFormation 에서 create 할 때 이 template 을 업로드* |
|:--:| 
| ![-]({{"/assets/220514/image4.png"| relative_url}}) | 
|:--:| 
| *해당 stack 이 생성됨* |
|:--:| 
| ![-]({{"/assets/220514/image5.png"| relative_url}}) | 
|:--:| 
| *Update 와 Delete 도 마찬가지의 과정으로 진행* |

---

Template 예시
```json
// json
{
    "AWSTemplateFormationVersion": "version date", // 2010-09-09 (optional)
    "Description": "JSON string", // optional
    "Metadata": { template metadata }, // optional
    "Parameters": { set of parameters }, // optional
    "Mapping": { set of mappings }, // optional
    "Conditions": { set of condition }, // optional
    "Transform": { set of transforms }, // optional
    "Resources": { // Required
        "MyEC2Instance": { // logical ID
            "Type": "AWS::EC2::Instance",
            "Properties": {
                "ImageId": "ami-12345678",
                "InstanceType": "t2.micro",
            }
        }
    },
    "Outputs": { set of outputs } // optional
}
```

Template 의 Resources 다른 예시
```yaml
# yaml
Resources:
    MyS3Bucket:
        Type: AWS::S3::Bucket
        Properties: 
            BucketName: HelloWorldWebsite
            AccessControl: PublicRead
            WebsiteConfiguration:
                IndexDocument: index.html
```

---

### Intrinsic functions
stack 을 관리할 때 사용할 수 있는 built-in functions 정리

#### 1. Join
```json
// json
{"Fn::Join": ["delemeter", [comma-delimited list of values]]}
// example
{"Fn::Join": [":", ["a", "b", "c"]]} // == "a:b:c"
```
```yaml
# yaml
!Join[delemeter,[comma-delimited list of values]]
# example
!Join [":", [ a, b, c]] # == "a:b:c"
```

#### 2. Ref
```json
// json
{"Ref": "AWS::Region"}
```
```yaml
# yaml
!Ref AWS::Region
```

#### 3. Mappings
```json
// json
{"Fn::FindInMap": ["MapName", "TopLevelKey", "SecondLevelKey"]}
{"Fn::FindInMap": ["RegionMap", "us-east-1", "AMI"]} // RegionMap 의 us-east-1 에 대한 AMI 값을 반환
```
```yaml
# yaml
!FindInMap [MapName, TopLevelKey, SecondLevelKey]
!FindInMap [RegionMap, us-east-1, AMI] # RegionMap 의 us-east-1 에 대한 AMI 값을 반환
```

#### 4. GetAtt
```json
// json
{"Fn::GetAtt": ["EC2Instance", "PublicDnsName"]}
```
```yaml
# yaml
!GetAtt
    - EC2Instance
    - PublicDnsName
```
---

### Multiple resources
CloudFormation 은 여러 자원을 다룰 때 유용함.
예시: EC2 with a security group to open port 22 (ssh)
기존에는 먼저 security group 을 port 22 로 열고, EC2 instance 가 이를 참조하게 해야함.
**CloudFormation 은 이러한 dependency 를 알아서 순서대로 처리해준다.**
아래와 같이 template 를 작성하면 된다.
```yaml
Resources:
  Ec2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: t2.micro
      ImageId: ami-43874721 # Amazon Linux AMI in Sydney
      Tags:
        - Key: "Name"
          Value: !Join [ " ", [ EC2, Instance, with, Fn, Join ] ]
      SecurityGroups: 
        - !Ref MySecurityGroup # 아래의 SecurityGroup 을 참조하도록 설정
  MySecurityGroup:
    Type: 'AWS::EC2::SecurityGroup'
    Properties:
      GroupDescription: Enable SSH access via port 22
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: '22'
          ToPort: '22'
          CidrIp: 0.0.0.0/0
```

---

### Pseudo parameters
Pseudo parameters 는 CloudFormation 에서 이미 지정해둔 매개변수들이다.
따라서 Template 에서 따로 선언할 필요 없다. (환경변수와 비슷하다)
Ref 라는 function 을 통해 참조할 수 있다.

#### pseudo parameters 예시:
```json
// json 에서는 "Ref": ~~ 라고 지정한다.
{
    "Resources": {
        "MySecurityGroup": {
            "Type": "AWS::EC2::SecurityGroup",
            "Properties": {
                "GroupDescription": {
                    "Ref": "AWS::Region"
                }
            }
        }
    }
}
```

```yaml
# yaml 에서는 !Ref ~~ 라고 지정한다.
Resources:
  Ec2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      InstanceType: t2.micro
      ImageId: ami-43874721 # Amazon Linux AMI in Sydney
      SecurityGroups: 
        - !Ref MySecurityGroup
      Tags:
        - Key: "Name"
          Value: !Join # 두 개의 문자열을 연결한다.
            - ""
            - - "EC2 Instance for "
              - !Ref AWS::Region # Ref 함수를 사용해 pseudo parameter 를 사용한 부분
  MySecurityGroup:
    Type: "AWS::EC2::SecurityGroup"
    Properties:
      GroupDescription: Enable SSH access via port 22
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: "22"
          ToPort: "22"
          CidrIp: 0.0.0.0/0
```

| ![-]({{"/assets/220514/image6.png"| relative_url}}) | 
|:--:| 
| *위의 yaml 파일을 사용해 EC2 instance 의 Name 을 EC2 instance for ap-southeast-2 로 바꾼 결과* |

#### 자주 사용되는 pseudo parameters:
**AWS::AccountId** : AWS 계정의 account ID 를 반환한다.
**AWS::NotificationARNs** : 현재 stack 의 notification ARNs 목록을 반환한다.
**AWS::StackId** : stack ID 를 반환한다.
**AWS::StackName** : stack name 을 반환한다.
**AWS::Region** : 자원이 생성된 AWS Region 을 나타내는 문자열을 반환한다.
그 외의 paramter 에 대한 설명은 다음 링크를 참조해보자: [**링크**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html)

### Mappings
Mapping 은 input 을 사용해 다른 값을 결정할 수 있게 만들어준다.
예를 들어 AMI ID 를 region 에 따라 결정하고자 할 때 Mapping 을 사용할 수 있다.
아래와 같이 Mappings 와 FindInMap 함수를 사용하면 Region 에 따라 올바른 AMI ID 를 사용해 instance 를 만들 수 있게 된다.
```json
// json
"Mappings": {
    "RegionMap": {
        "us-east-1": {"AMI": "ami-78293f291"},
        "us-west-1": {"AMI": "ami-6552930fa"}
    }
    // 위와 같이 설정된 key-value 가 있을 때
}
...
"ImageId": {
    "Fn::FindInMap": ["RegionMap", {"Ref": "AWS::Region"}, "AMI"]
    // RegionMap 의 "us-east-1" 에서 AMI 값을 반환한다.
}
```
```yaml
# yaml
Mappings:
    RegionMap:
        us-east-1:
            AMI: ami-78293f291
        us-west-1:
            AMI: ami-6552930fa
...
Resources:
    Ec2Instance:
        Type: 'AWS::EC2::Instance'
        Properties:
            ImageId: !FindInMap
                - RegionMap
                - !Ref 'AWS::Region'
                - AMI
```

---

### Input Parameters
Input Parameters 는 template 에 custome value 를 입력할 수 있게 해준다.
이들은 top level Parameters 부분에서 정의된다.
각 parameter 는 하나의 값이 할당되어 있어야 한다.
default value 를 설정할 수도 있다.
필요한 유일한 attribute 는 데이터 타입을 나타내는 "Type" 이다.

지원되는 Type 의 목록:
- String
- Number
- List<Number>
- CommaDelimitedList
- AWS-specific types (AWS::EC2::Image::Id 등)
- Systems MAnager Parameter types

Input parameters 를 사용한 예시:
```json
// json
"Parameters": {
    "InstTypeParam": {
        "Type": "String",
        "Default": "t2.micro",
        "AllowedValues": [
            "T2.micro",
            "m1.small",
            "m1.large"
        ],
        "Description": "EC2 Instance Type"
    }
}
...
"Resources": {
    "Ec2Instance": {
        "Type": "AWS::EC2::Instance",
        "Properties": {
            "InstanceType": {
                "Ref": "InstTypeParam"
            },
            "ImageId": "ami-2f726546"
        }
    }
}
```
```yaml
# yaml
Parameters: # 사용하고자 하는 paramters 를 정의함.
  NameOfService:
    Description: "The name of the service this stack is to be used for."
    Type: String
  KeyName:
    Description: Name of an existing EC2 KeyPair to enable SSH access into the server
    Type: AWS::EC2::KeyPair::KeyName
Resources:
  Ec2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: t2.micro
      ImageId:
        Fn::FindInMap:
        - RegionMap
        - !Ref AWS::Region
        - AMI
      SecurityGroups: 
        - !Ref MySecurityGroup
      Tags:
        - Key: "Name"
          Value: !Ref NameOfService # Parameters 에서 정의한 input parameters 를 사용함. 
```
아래는 위 yaml 파일에서 지정한 "KeyNAme" 의 AWS::EC2::KeyPair::KeyName 가 사용된 예시이다.

| ![-]({{"/assets/220514/image7.png"| relative_url}}) | 
|:--:| 
| *먼저 EC2 에서 key-pair 가 만들어져 있어야 함. EC2 -> Key pair 에서 Create 로 "Test key pair" 를 만든 뒤,* |
| ![-]({{"/assets/220514/image8.png"| relative_url}}) | 
|:--:| 
| *CloudFormation 으로 와서 위 yaml template 으로 update 를 시도하면, 기존에 없었던 KeyName 선택 창이 나타난다.* |

---

### Outputs
Outputs 는 stack 에 있는 자원의 정보에 접근할 수 있게 해준다.
예를 들어, output 을 사용해 EC2 instance 가 생성될 때 PublicIP or DNA 를 반환하도록 할 수 있다.
Outputs 역시 Input 과 마찬가지로 template top level 에 정의한다.

![-]({{"/assets/220514/image9.png"| relative_url}})

예시:
```json
// json
"Outputs": {
    "InstanceDns": {
        "Description": "The Instance Dns",
        "Value": {
            "Fn::GetAtt": [
                "EC2Instance",
                "PublicDnsName"
            ]
        }
    }
}
```
```yaml
# yaml
Outputs:
  ServerDns:
    Value: !GetAtt
      - Ec2Instance
      - PublicDnsName
```
위의 예시에 정의된 InstanceDns, ServerDns 등은 dynamic 하기 때문에 생성되기 전에는 알 수 없다.
따라서 위와 같이 Outputs 에 정의하면 생성될 때 해당 변수에 저장되어 사용할 수 있게 된다.
위 yaml template 을 사용해 cloudFormation 에서 update 를 하면, 아래와 같이 Outputs 탭에서 새로운 key 와 value 가 만들어진 것을 확인할 수 있다.

| ![-]({{"/assets/220514/image10.png"| relative_url}}) | 
