
## test 02 ? !


This is my **002 Docusaurus document**!

---

![](docusaurus-plushie-banner.jpeg)

```

Multicapacitor (v1.15.9) Latest

@fjl fjl released this 3 days ago

 v1.15.9

 74165a8

This release enables the Prague execution-layer fork on mainnet.

  

Prague

As of this release, the Prague fork is scheduled to occur on mainnet at block timestamp 1746612311 (Wed May 07 10:05:11 2025 UTC). As a reminder, the fork contains the following EIPs:

  

EIP-2537: Precompile for BLS12-381 curve operations

EIP-2935: Save historical block hashes in state

EIP-6110: Supply validator deposits on chain

EIP-7002: Execution layer triggerable exits

EIP-7251: Increase the MAX_EFFECTIVE_BALANCE

EIP-7549: Move committee index outside Attestation

EIP-7623: Increase calldata cost

EIP-7685: General purpose execution layer requests

EIP-7691: Blob throughput increase

EIP-7702: Set EOA account code

EIP-7840: Add blob schedule to EL config files

All changes

The Prague fork timestamp was added for mainnet. (#31535)

Transaction-sending RPCs will now add txs to the 'locals' tracker only when they have any chance of inclusion. This is a bit of a revert from the behavior we added in v1.15.4, where APIs such as eth_sendRawTransaction would always return a txhash, even if the transaction wasn't includable on chain. (#31618)

If an EVM system call fails during block execution, the block is considered invalid. (#31639)

An corner-case crash in eth_feeHistory related to blob fees is resolved. (#31663)

Several correctness bugs in the new log indexer have been fixed. (#31590, #31680, #31671, #31668, #31642)

The history pruning implementation was further improved. (#31638, #31636, #31656)

Geth will now print periodic logs when a non-activated fork is configured. (#31340)

Geth will now occasionally drop peers at random after being fully synced. (#31476)

CPU usage of tx propagation has been optimized. (#31657)

Peer disconnect metrics are improved. (#31629, #31621)

For a full rundown of the changes please consult the Geth 1.15.9 release milestone

  

As with all our previous releases, you can find the:

  

Pre-built binaries for all platforms on our downloads page.

Docker images published under ethereum/client-go.

Ubuntu packages in our Launchpad PPA repository.

OSX packages in our Homebrew Tap repository.

  

  

v6.0.0 Latest

@prestonvanloon prestonvanloon released this 2 days ago

· 15 commits to develop since this release

 v6.0.0

 a007182

v6.0.0 - Pectra Mainnet!!!

This release introduces Mainnet support for the upcoming Electra + Prague (Pectra) fork. The fork is scheduled for mainnet epoch 364032 (May 7, 2025, 10:05:11 UTC). You MUST update Prysm Beacon Node, Prysm Validator Client, and your execution layer client to the Pectra ready release prior to the fork to stay on the correct chain.

  

Besides Pectra, we have more light client API support, cleanups, and a few bugfixes. Please review the changelog below and update your client as soon as practical before May 7.

  

This release is mandatory for all operators before May 7.

  

Added

Implemented validator identities Beacon API endpoint. [PR]

Add SSZ support to light client updates by range API. [PR]

Add light client ssz types to the spec test. [PR]

Added the ability for execution requests to be tested in e2e with electra. [PR]

Add warning messages for gas limit ranges that might be problematic. Low gas limits (≤10% of default) may cause transactions to fail, while high gas limits (>150% of default) could lead to block propagation issues. [PR]

Add light client store object to the beacon node object. [PR]

prysmctl option in wrapper script to generate devnet ssz. [PR]

Add support for Electra fork epoch. [PR]

Changed

The validator client will no longer use the full list of committee values but instead use the committee length and validator committee index. [PR]

Remove the header Content-Disposition from the httputil.WriteSSZ function. No filename parameter is needed anymore. [PR]

Sort attestations in proposer block by reward. [PR]

More efficient query method for stategen to retrieve blocks between a given state and the replay target block. This avoids attempting to look up blocks that are not needed for head replay queries, which may be missing due to a previous rollback bug. [PR]

removed old web3signer metrics in favor for a universal one. [PR]

Deprecated everything related with the gRPC API. [PR]

Migrate Prysm repo to Offchain Labs organization ahead of Pectra upgrade v6. [PR]

Deprecated

deprecates and removes usage of the --trace flag and--cpuprofile flag in favor of just using --pprof. [PR]

Removed

Remove /eth/v1/beacon/states/head/committees call when getting duties. [PR]

Removed unused hack scripts. [PR]

Remove disable-committee-aware-packing flag. [PR]

Remove deprecated flags for the major release. [PR]

Removed Beacon API endpoints which have been deprecated at the Deneb fork. [PR]

Fixed

The --rpc flag will now properly enable the keymanager APIs without web. The --web will enable both validator api endpoints and web. [PR]

Use latest state to pack attestation. [PR]

Clean up dangling block index entries for blocks that were previously deleted by incomplete cleanup code. [PR]

Fixed to use io stream instead of stream read. [PR]

When using a DV, send all aggregations for a slot and committee. [PR]

Fixed a bug in consolidation request processing. [PR]

Fix State Getter for pending withdrawal balance. [PR]

Fixed a bug in checking for attestation lengths in our block. [PR]

Fix Committee Index Check For Aggregates. [PR]

Fix filtering by committee index post-Electra in ListAttestationsV2. [PR]

Peers giving invalid data in range syncing are now downscored. [PR]

Adding fork guard to attestation api endpoints so that it doesn't accidentally include wrong attestation types in the pool. [PR]

fixed underflow with balances in leaking edge case with expected withdrawals. [PR]

Attribute block and blob issues to correct peers during range syncing. [PR]
```

---

## **🔧** 

## **执行层更新：Geth v1.15.9**

  

### **🌍** 

### **主网激活 Prague 分叉**

- **生效时间：2025年5月7日 10:05:11 UTC**
    
- 包含的 EIP（提案）如下：
    
    1. **EIP-2537**：为 BLS12-381 曲线操作添加预编译合约（用于 zk 与 ETH2 验证优化）
        
    2. **EIP-2935**：将历史区块哈希存入状态（更好支持轻客户端）
        
    3. **EIP-6110**：链上提供验证者押金信息（与信标链更好协作）
        
    4. **EIP-7002**：允许从执行层触发验证者退出
        
    5. **EIP-7251**：提高 MAX_EFFECTIVE_BALANCE（让大额质押更有效率）
        
    6. **EIP-7549**：从 Attestation 中移除委员会索引（简化结构）
        
    7. **EIP-7623**：增加 calldata 成本（可能抑制 spam）
        
    8. **EIP-7685**：更通用的执行层请求支持
        
    9. **EIP-7691**：提高 blob 吞吐量（配合 proto-danksharding）
        
    10. **EIP-7702**：给 EOA 账户设置代码（未来可升级账户模型）
        
    11. **EIP-7840**：将 blob 时间表配置加入执行层 config 文件
        
    

  

### **🛠 其他更新**

- 修复了多项 bug（包括 blob 费用问题、日志索引问题）
    
- 优化了历史修剪（pruning）、CPU 使用效率、peer 断开等细节
    
- 改变了交易 RPC 的行为：只有可能被打包的交易才会返回 txhash
    
- 新增日志提示：如果配置了但未激活某分叉，会有日志提醒
    

---

## **🔗** 

## **共识层更新：Prysm v6.0.0**

  

### **⚡** 

### **支持 Pectra 分叉**

- 时间与执行层同步：2025年5月7日 10:05:11 UTC
    
- 强制更新，**所有节点运营者必须升级**以避免分叉
    
- 包含 Electra 的共识层部分变化
    

  

### **🚀 主要新增**

- Light Client API 的更多支持（SSZ 类型、状态对象、范围更新等）
    
- Electra epoch 支持及相关测试工具
    
- 增加 gas limit 范围提示（过低/过高会有风险提示）
    
- 增加对 Electra 执行请求的 e2e 测试支持
    

  

### **🧹 清理与优化**

- 大量废弃旧接口和无用脚本（gRPC API、旧 flags、旧 metrics）
    
- 更高效的状态回放处理逻辑
    
- 简化了 attestation 的委员会结构使用
    
- 排序机制优化（按奖励排列）
    

  

### **🐞 Bug修复**

- 各种同步、验证、打包、范围查询相关的修复
    
- 提升错误处理与 peers 的评分机制，避免无效数据干扰
    

---

## **✅ 总结：这次 Pectra 分叉重点**

1. **连接以太坊1.0 和 2.0 的关键桥梁**
    
2. **为 proto-danksharding、账户抽象、轻客户端 打基础**
    
3. **优化链上验证者交互与数据结构**
    
4. **提高网络吞吐与效率，打击垃圾交易**
    

  
