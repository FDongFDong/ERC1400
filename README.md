![Codefi](images/CodefiBanner.png)

> https://github.com/ndaxio/ERC1400

해당 프로젝트는 스터디 차원에서 한글화하여 진행하겠습니다.


# 소개

블록체인 기술, 특히 '프로그래밍 가능한' 토큰의 등장은 금융 자산에 대한 새로운 가능성, 즉 디지털 자산의 창출이라는 세계를 열었습니다.
디지털 자산은 "토큰화"된 금융 자산입니다. 즉, 각 자산은 블록체인에서 토큰으로 표시됩니다.

이러한 토큰은 여러 표준으로 표현할 수 있습니다:
 - **ERC20** 는 가장 기본적이고 가장 많이 채택된 토큰 표준입니다. "토큰 표준의 공리"라고 할 수 있으며, 현존하는 대부분의 도구 및 플랫폼과 호환됩니다.
 - **ERC1400** 는 보다 진화된 표준으로, ERC20을 준수하며 토큰화된 금융 자산의 사용 사례에 맞게 정밀하게 설계되어 고도로 제어 가능한 토큰 전송을 수행할 수 있습니다.

다음 리포지토리에는 Codefi Assets 플랫폼에서 사용하는 ERC1400 구현이 포함되어 있습니다.


#Codefi Assets이란 무엇인가요?

Codefi Assets은 이더리움 블록체인을 기반으로 토큰화된 금융 자산을 발행하고 관리할 수 있는 고급 기관 기술 플랫폼입니다. Codefi Assets은 컨센시스에서 만든 제품입니다.
https://codefi.consensys.net/codefiassets

### 금융 자산 발행 및 관리를 위한 플랫폼

현재 자본 시장은 여전히 몇 가지 문제점을 극복해야 합니다:
 - 오늘날 자산을 발행하는 것은 번거롭고 비용이 많이 듭니다.
 - 발행된 자산은 주로 고액 투자자를 위해 예약되어 있습니다.
 - 이러한 자산은 쉽게 거래할 수 없기 때문에 secondary 마켓의 가능성이 크게 제한됩니다.

With Codefi Assets, 저희는 이러한 문제점을 해결하기 위해 자본 시장을 토큰화하고자 합니다. 새로운 시스템에서는 이렇게 상상합니다:
 - 자산 발행이 지금보다 더 빠르고 간편해질 뿐만 아니라 비용도 절감됩니다.
 - 이렇게 비용을 절감하면 소규모 티켓 투자자를 온보딩할 수 있습니다.
 - 전 세계적으로 토큰화는 시장에 대한 강력한 통제력을 유지하면서 보다 유동적이고 마찰 없는 자산 이전을 위해 제약을 제거하여 secondary 마켓을 자유롭게 합니다.

### Codefi Assets 기술 기반의 자산 발행 플랫폼 데모 영상

![CodefiVideo](images/CodefiVideo.png)

Link to video:
https://www.youtube.com/watch?v=PjunjtIj02c


# 토큰 표준에 대한 간략한 개요 (ERC20, ERC1400) 

![Picture1](images/Picture1.png)
![Picture2](images/Picture2.png)
![Picture3](images/Picture3.png)
![Picture4](images/Picture4.png)


# CodeFi Asset에서 ERC1400 토큰 표준을 사용하는 이유는 무엇인가요?

ERC1400은 ERC20 기능에 추가 기능을 도입하고 발행자에게 금융 자산에 대한 강력한 제어 기능을 제공합니다.

### Introduction - The limits of ERC20 token standard

현재 암호화폐 커뮤니티에서 가장 일반적이고 잘 알려진 표준은 ERC20입니다.([eips.ethereum.org/EIPS/eip-20](https://eips.ethereum.org/EIPS/eip-20)).
대다수의 ICO가 이 ERC20 표준을 기반으로 하지만, 금융 자산 토큰화에 가장 적합한 표준은 아닌 것으로 보입니다.
ERC20 토큰 전송을 수행하는 데 필요한 매개변수는 수취인의 주소와 전송 값뿐이므로 전송에 대한 제어 가능성이 제한됩니다:
```
function transfer(address recipient, uint256 value)
```
모든 컨트롤은 on-chain에서 hard-code되어야 하며, 투자자가 블랙리스트에 있는지 여부를 확인하는 등 단순/바이너리 검사로 제한되는 경우가 많습니다.

Codefi Assets은 더욱 진화하고 세분화된 컨트롤을 사용하여 전송을 보호합니다.

이러한 컨트롤은 빠르게 진화할 수 있고 유연성이 필요하기 때문에 on-chain에서 hard-code하기 어렵습니다.

### 인증서 기반 전송 제어 - 하나의 단일 트랜잭션에서 다중 서명을 수행하는 방법

The use of an additional 'data' parameter in the transfer functions can enable more evolved/granular controls:
```
function transferWithData(address recipient, uint256 value, bytes data)
```
Codefi Assets fosters to use this additional 'data' field, available in the ERC1400 standard, in order to inject a certificate generated off-chain by the issuer.
A token transfer shall be conditioned to the validity of the certificate, thus offering the issuer with strong control capabilities over its financial assets.

![Picture5](images/Picture5.png)

The Codefi certificate contains:
 - The function ID which ensures the certificate can’t be used on an other function.
 - The parameters which ensures the input parameters have been validated by the issuer.
 - A validity date which ensures the certificate can’t be used after validity date.
 - A nonce which ensures the certificate can’t be used twice.

Finally the certificate is signed by the issuer which ensures it is authentic.

The certificate enables the issuer to perform advanced conditional ownership, since he needs to be aware of all parameters of a transaction before generating the associated certificate.

![Picture6](images/Picture6.png)

In a way, this can be seen as a way to perform multisignature in one single transaction since every asset transfer requires:
 - A valid transaction signature (signed by the investor)
 - A valid certificate signature (signed by the issuer)

 ### Example use case
 
 An example use case for the certificate validation is KYC verification.

 The certificate generator can be coupled to a KYC API, and only provide certificates to users who've completed their KYC verification before.

 ![Picture7](images/Picture7.png)

PS: Since the ERC1400 standard is agnostic about the way to control certificate, we didn't include our certificate controller in this repository (a mock is used instead). In order to perform real advanced conditional ownership, a certificate controller called 'CertificateController.sol' shall be placed in folder '/contracts/CertificateController' instead of the mock placed there.


# Description of ERC1400 standard

ERC1400 introduces new concepts on top of ERC20 token standard:
 - **Granular transfer controls**: Possibility to perform granular controls on the transfers with a system of certificates (injected in the additional `data` field of the transfer method)
 - **Controllers**: Empowerment of controllers with the ability to send tokens on behalf of other addresses (e.g. force transfer).
 - **Partionned tokens** (partial-fungibility): Every ERC1400 token can be partitioned. The partition of a token, can be seen as the state of a token. It is well adapted for representing, classes of assets, performing corporate actions, etc.
 - **Document management**: Possibility to bind tokens to hashes of legal documents, thus making the link between a blockchain transaction and the real world.

Optionally, the following features can also be added:
 - **Hooks**: Possibility for token senders/recipients to setup hooks, e.g. automated actions executed everytime they send/receive tokens, thanks to [ERC1820](http://eips.ethereum.org/EIPS/eip-1820).
 - **Upgradeability**: Use of ERC1820([eips.ethereum.org/EIPS/eip-1820](http://eips.ethereum.org/EIPS/eip-1820)) as central contract registry to follow smart contract migrations.


# Focus on ERC1400 implementation choices

The original submission with discussion can be found at: [github.com/ethereum/EIPs/issues/1411](https://github.com/ethereum/EIPs/issues/1411).

We've performed a few updates compared to the original submission, mainly to fit with business requirements + to save gas cost of contract deployment.

#### Choices made to fit with business requirements
 - Introduction of sender/recipient hooks ([IERC1400TokensRecipient](contracts/token/ERC1400Raw/IERC1400TokensRecipient.sol), [IERC1400TokensSender](contracts/token/ERC1400Raw/IERC1400TokensSender.sol)). Those are inspired by [ERC777 hooks]((https://eips.ethereum.org/EIPS/eip-777)), but they have been updated in order to support partitions, in order to become ERC1400-compliant.
 - Modification of view functions ('canTransferByPartition', 'canOperatorTransferByPartition') as consequence of our certificate design choice: the view functions need to have the exact same parameters as 'transferByPartition' and 'operatorTransferByPartition' in order to be in measure to confirm the certificate's validity.
 - Introduction of validator hook ([IERC1400TokensValidator](contracts/token/ERC1400Raw/IERC1400TokensValidator.sol)), to manage updates of the transfer validation policy across time (certificate, whitelist, blacklist, lock-up periods, investor caps, pauseability, etc.), thanks an upgradeable module.
 - Extension of ERC20's allowance feature to support partitions, in order to become ERC1400-compliant. This is particularly important for secondary market and delivery-vs-payment.
 - Possibility to migrate contract, and register new address in ERC1820 central registry, for smart contract upgradeability.

#### Choices made to save gas cost of contract deployment
 - Removal of controller functions ('controllerTransfer' and 'controllerRedeem') and events ('ControllerTransfer' and 'ControllerRedemption') to save gas cost of contract deployment. Those controller functionalities have been included in 'operatorTransferByPartition' and 'operatorRedeemByPartition' functions instead.
 - Export of 'canTransferByPartition' and 'canOperatorTransferByPartition' in optional checker hook [IERC1400TokensChecker](contracts/token/ERC1400Raw/IERC1400TokensChecker.sol) as those functions take a lot of place, although they are not essential, as the result they return can be deduced by calling other view functions of the contract.


# Interfaces

For better readability, ER1400 contract has been structured into different parts:
 - [ERC1400Raw](contracts/token/ERC1400Raw/ERC1400Raw.sol), contains the minimum logic, recommanded to manage financial assets: granular transfer controls with certificate, controllers, hooks, migrations
 - [ERC1400Partition](contracts/token/ERCC1400Partition/ERC1400Partition.sol), introduces the concept of partitionned tokens (partial fungibility)
 - [ERC1400](contracts/token/ERC1400.sol), adds the issuance/redemption logic
 - [ERC1400ERC20](contracts/token/ERC20/ERC1400ERC20.sol), adds the ERC20 backwards compatibility

### ERC1400Raw interface

ERC1400Raw can be used:
 - Either as a sub-contract of ERC1400Partition
 - Or as a standalone contract, in case partitions are not required for the token

```
interface IERC1400Raw {

  // Token Information
  function name() external view returns (string);
  function symbol() external view returns (string);
  function totalSupply() external view returns (uint256);
  function balanceOf(address owner) external view returns (uint256);
  function granularity() external view returns (uint256);

  // Operators
  function controllers() external view returns (address[]);
  function authorizeOperator(address operator) external;
  function revokeOperator(address operator) external;
  function isOperator(address operator, address tokenHolder) external view returns (bool);
  event AuthorizedOperator(address indexed operator, address indexed tokenHolder);
  event RevokedOperator(address indexed operator, address indexed tokenHolder);

  // Token Transfers
  function transferWithData(address to, uint256 value, bytes data) external;
  function transferFromWithData(address from, address to, uint256 value, bytes data, bytes operatorData) external;

  // Token Issuance/Redemption
  function redeem(uint256 value, bytes data) external;
  function redeemFrom(address from, uint256 value, bytes data, bytes operatorData) external;
  event Issued(address indexed operator, address indexed to, uint256 value, bytes data, bytes operatorData);
  event Redeemed(address indexed operator, address indexed from, uint256 value, bytes data, bytes operatorData);

}
```

### ERC1400Partition interface

ERC1400Partition adds an additional feature on top of ERC1400Raw properties: the possibility to partition tokens (partial-fungibility property).
This property allows to perform corporate actions, like mergers and acquisitions, which is essential for financial assets.

```
interface IERC1400Partition {

    // Token Information
    function balanceOfByPartition(bytes32 partition, address tokenHolder) external view returns (uint256);
    function partitionsOf(address tokenHolder) external view returns (bytes32[]);

    // Token Transfers
    function transferByPartition(bytes32 partition, address to, uint256 value, bytes data) external returns (bytes32);
    function operatorTransferByPartition(bytes32 partition, address from, address to, uint256 value, bytes data, bytes operatorData) external returns (bytes32);
    event TransferByPartition(
        bytes32 indexed fromPartition,
        address operator,
        address indexed from,
        address indexed to,
        uint256 value,
        bytes data,
        bytes operatorData
    );
    event ChangedPartition(
        bytes32 indexed fromPartition,
        bytes32 indexed toPartition,
        uint256 value
    );

    // Default Partition Management
    function getDefaultPartitions(address tokenHolder) external view returns (bytes32[]);
    function setDefaultPartitions(bytes32[] partitions) external;

    // Operators
    function controllersByPartition(bytes32 partition) external view returns (address[]);
    function authorizeOperatorByPartition(bytes32 partition, address operator) external;
    function revokeOperatorByPartition(bytes32 partition, address operator) external;
    function isOperatorForPartition(bytes32 partition, address operator, address tokenHolder) external view returns (bool);
    event AuthorizedOperatorByPartition(bytes32 indexed partition, address indexed operator, address indexed tokenHolder);
    event RevokedOperatorByPartition(bytes32 indexed partition, address indexed operator, address indexed tokenHolder);

}
```

### ERC1400 interface

ERC1400 adds issuance/redemption + document management logics upon ERC1400Partition.
```
interface IERC1400 {

    // Document Management
    function getDocument(bytes32 name) external view returns (string, bytes32);
    function setDocument(bytes32 name, string uri, bytes32 documentHash) external;
    event Document(bytes32 indexed name, string uri, bytes32 documentHash);

    // Controller Operation
    function isControllable() external view returns (bool);

    // Token Issuance
    function isIssuable() external view returns (bool);
    function issueByPartition(bytes32 partition, address tokenHolder, uint256 value, bytes data) external;
    event IssuedByPartition(bytes32 indexed partition, address indexed operator, address indexed to, uint256 value, bytes data, bytes operatorData);

    // Token Redemption
    function redeemByPartition(bytes32 partition, uint256 value, bytes data) external;
    function operatorRedeemByPartition(bytes32 partition, address tokenHolder, uint256 value, bytes data, bytes operatorData) external;
    event RedeemedByPartition(bytes32 indexed partition, address indexed operator, address indexed from, uint256 value, bytes data, bytes operatorData);

    // Transfer Validity
    function canTransferByPartition(bytes32 partition, address to, uint256 value, bytes data) external view returns (byte, bytes32, bytes32);
    function canOperatorTransferByPartition(bytes32 partition, address from, address to, uint256 value, bytes data, bytes operatorData) external view returns (byte, bytes32, bytes32);

}
```

### ERC1400ERC20 interface

Finally ERC1400ERC20 introduces the last missing layer: ERC20 backwards compatibility. It is not mandatory but quite essential, because it ensures the token is compatible with all ERC20-compliant platforms.

```
interface IERC20 {

    // Token Information
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);

    // Token Transfers
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);

    // Allowances
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    event Approval(address indexed owner, address indexed spender, uint256 value);

}
```

NB: [ERC1400RawERC20](contracts/token/ERC20/ERC1400RawERC20.sol) has been created in case ERC20 backwards retrocompatibility is required, but not the partitions.


## Quick start: How to test the contract?

Prerequisites: please make sure you installed "yarn" on your environment.
```
$ brew install yarn
```

Make sure your node version is <=10.x.x. (yarn command fails for more recent node versions).

Test the smart contract, by running the following commands:
```
$ git clone git@github.com:ConsenSys/ERC1400.git
$ cd ERC1400
$ yarn
$ yarn coverage
```


## How to deploy the contract on a blokchain network?

#### Step1: Define Ethereum wallet and Ethereum network to use in ".env" file

A few environment variables need to be specified. Those can be added to a ".env" file: a template of it can be generated with the following command:
```
$ yarn env
```

The ".env" template contains the following variables:

MNEMONIC - Ethereum wallets which will be used by the webservice to sign the transactions - [**MANDATORY**] (see section "How to get a MNEMONIC?" in appendix)

INFURA_API_KEY - Key to access an Ethereum node via Infura service (for connection to mainnet or ropsten network) - [OPTIONAL - Only required if NETWORK = mainnet/ropsten] (see section "How to get an INFURA_API_KEY?" in appendix)

#### Step2: Deploy contract

**Deploy contract on ganache**

In case ganache is not installed:
```
$ yarn global add ganache-cli
```
Then launch ganache:
```
$ ganache-cli
```

In a different console, deploy the contract by running the migration script:
```
$ yarn truffle migrate
```

**Deploy contract on ropsten**

Deploy the contract by running the migration script:
```
$ yarn truffle migrate --network ropsten
```


## APPENDIX

### How to get a MNEMONIC?

#### 1.Find a MNEMONIC

There are 2 options to get MNEMONIC:
 - Either generate 12 random words on https://iancoleman.io/bip39/ (BIP39 Mnemonic).
 - Or get the MNEMONIC generated by ganache with the following command:
```
$ ganache-cli
```
The second option is recommended for development purposes since the wallets associated to the MNEMONIC will be pre-loaded with ETH for tests on ganache.

#### 2.Load the wallet associated to the MNEMONIC with ether

If you've used ganache to generate your MNEMONIC and you only want to perform tests on ganache, you have nothing to do. The accounts are already loaded with 100 ETH.

For all other networks than ganache, you have to send ether to the accounts associated to the MNEMONIC:
 - Discover the accounts associated to your MNEMONIC thanks to https://www.myetherwallet.com/#view-wallet-info > Mnemonic phrase.
 - Send ether to those accounts.

### How to get an INFURA_API_KEY?

INFURA_API_KEY can be generated by creating an account on https://infura.io/
