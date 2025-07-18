# 主数据管理流程图 - Mermaid格式

## 完整流程图

```mermaid
graph TD
    %% 外部系统区域
    subgraph "外部系统"
        A1[买卖双方签订合同]
        A2[订舱客户及人]
        A3[承运客户及人]
        A4[客户合同/交易关系]
    end

    %% 核心管理分发网络
    subgraph "管理分发网络"
        B1[供应商管理]
        B2[产品管理]
        B3[供应商档案]
        B4[商品档案]
        B5[网点档案]
    end

    %% 产品相关
    subgraph "产品管理模块"
        C1[产品开票/人员体系]
        C2[产品库]
        C3[产品]
        C4[供应服务]
        C5[供应商台账]
        C6[采购采购方式审批流程]
    end

    %% 业务数据处理区域
    subgraph "核心业务数据"
        D1[实际]
        D2[功能]
        D3[结论]
        D4[国外数据]
        D5[业务分录]
        D6[服务分录]
        D7[应收账]
    end

    %% 基础数据表
    subgraph "基础数据"
        E1[职位表]
        E2[签折表]
        E3[签折量]
    end

    %% 右侧管理功能
    subgraph "管理功能"
        F1[权限控制]
        F2[数据同步]
        F3[外部交互]
    end

    %% 数据流向
    A1 --> B1
    A2 --> B2
    A3 --> B3
    A4 --> B4
    
    B1 --> C1
    B2 --> C2
    B3 --> C3
    B4 --> C4
    B5 --> C5
    
    C1 --> D1
    C2 --> D2
    C3 --> D3
    C4 --> D4
    C5 --> D5
    C6 --> D6
    
    D1 --> E1
    D2 --> E2
    D3 --> E3
    
    B1 --> F1
    B2 --> F2
    B3 --> F3
    
    %% 样式定义
    classDef external fill:#e1f5fe
    classDef management fill:#f3e5f5
    classDef product fill:#e8f5e8
    classDef business fill:#fff3e0
    classDef basic fill:#f1f8e9
    classDef function fill:#fce4ec
    
    class A1,A2,A3,A4 external
    class B1,B2,B3,B4,B5 management
    class C1,C2,C3,C4,C5,C6 product
    class D1,D2,D3,D4,D5,D6,D7 business
    class E1,E2,E3 basic
    class F1,F2,F3 function
```

## 详细子流程

### 1. 外部系统接入流程
```mermaid
sequenceDiagram
    participant 外部系统
    participant 管理分发网络
    participant 核心业务系统
    
    外部系统->>管理分发网络: 提交合同/客户信息
    管理分发网络->>管理分发网络: 数据验证与标准化
    管理分发网络->>核心业务系统: 分发标准化数据
    核心业务系统->>管理分发网络: 返回处理结果
    管理分发网络->>外部系统: 反馈处理状态
```

### 2. 供应商管理流程
```mermaid
flowchart LR
    subgraph "供应商管理"
        A[供应商信息录入] --> B[信息验证]
        B --> C[档案建立]
        C --> D[供应商评级]
        D --> E[供应商台账]
    end
    
    subgraph "产品管理"
        F[产品信息] --> G[产品分类]
        G --> H[产品档案]
        H --> I[产品库]
    end
    
    E --> J[采购审批]
    I --> J
    J --> K[采购执行]
```

### 3. 数据分发架构
```mermaid
graph TB
    subgraph "数据源"
        DS1[外部合同系统]
        DS2[客户管理系统]
        DS3[供应商系统]
    end
    
    subgraph "管理分发网络"
        MDN[主数据管理平台]
        MDN --> |数据清洗| DC[数据清洗]
        MDN --> |数据标准化| DS[数据标准化]
        MDN --> |数据验证| DV[数据验证]
    end
    
    subgraph "目标系统"
        TS1[产品管理系统]
        TS2[供应链系统]
        TS3[财务系统]
        TS4[业务系统]
    end
    
    DS1 --> MDN
    DS2 --> MDN
    DS3 --> MDN
    
    DC --> TS1
    DS --> TS2
    DV --> TS3
    MDN --> TS4
```

## 关键数据实体

### 核心实体关系图
```mermaid
erDiagram
    合同 ||--o{ 客户 : 签订
    客户 ||--o{ 订单 : 下达
    订单 ||--o{ 产品 : 包含
    产品 ||--o{ 供应商 : 供应
    供应商 ||--o{ 档案 : 维护
    
    合同 {
        string 合同编号
        string 合同名称
        date 签订日期
        string 合同状态
    }
    
    客户 {
        string 客户编号
        string 客户名称
        string 客户类型
        string 联系方式
    }
    
    产品 {
        string 产品编号
        string 产品名称
        string 产品分类
        decimal 产品价格
    }
    
    供应商 {
        string 供应商编号
        string 供应商名称
        string 供应商等级
        string 联系方式
    }
```

## 业务规则说明

1. **数据流向规则**：
   - 所有外部数据必须通过管理分发网络进行标准化
   - 核心业务系统不直接与外部系统交互
   - 数据变更需要经过审批流程

2. **权限控制规则**：
   - 不同角色有不同的数据访问权限
   - 关键数据修改需要审批
   - 数据导出需要授权

3. **数据质量规则**：
   - 数据入库前必须进行验证
   - 重复数据自动识别和处理
   - 数据变更全程可追溯
