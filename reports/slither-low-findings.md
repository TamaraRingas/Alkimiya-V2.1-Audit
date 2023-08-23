Summary
 - [shadowing-local](#shadowing-local) (3 results) (Low)
 - [missing-zero-check](#missing-zero-check) (1 results) (Low)
 - [calls-loop](#calls-loop) (1 results) (Low)
 - [reentrancy-benign](#reentrancy-benign) (2 results) (Low)
 - [reentrancy-events](#reentrancy-events) (11 results) (Low)
 - [timestamp](#timestamp) (3 results) (Low)
 - [constable-states](#constable-states) (2 results) (Optimization)
## shadowing-local
Impact: Low
Confidence: High
 - [ ] ID-0
[AbstractSilicaV2_1.getDaysAndRewardFulfilled().rewardDelivered](contracts/AbstractSilicaV2_1.sol#L324) shadows:
        - [SilicaV2_1Storage.rewardDelivered](contracts/storage/SilicaV2_1Storage.sol#L22) (state variable)

contracts/AbstractSilicaV2_1.sol#L324


 - [ ] ID-1
[AbstractSilicaV2_1._deposit(address,address,uint256,uint256)._totalSupply](contracts/AbstractSilicaV2_1.sol#L492) shadows:
        - [ERC20._totalSupply](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L43) (state variable)

contracts/AbstractSilicaV2_1.sol#L492


 - [ ] ID-2
[AbstractSilicaV2_1._getDaysAndRewardFulfilled(uint256,uint256,uint256).rewardDelivered](contracts/AbstractSilicaV2_1.sol#L338) shadows:
        - [SilicaV2_1Storage.rewardDelivered](contracts/storage/SilicaV2_1Storage.sol#L22) (state variable)

contracts/AbstractSilicaV2_1.sol#L338


## missing-zero-check
Impact: Low
Confidence: Medium
 - [ ] ID-3
[Ownable2Step.transferOwnership(address).newOwner](node_modules/@openzeppelin/contracts/access/Ownable2Step.sol#L35) lacks a zero-check on :
                - [_pendingOwner = newOwner](node_modules/@openzeppelin/contracts/access/Ownable2Step.sol#L36)

node_modules/@openzeppelin/contracts/access/Ownable2Step.sol#L35


## calls-loop
Impact: Low
Confidence: Medium
 - [ ] ID-4
[Address.functionCallWithValue(address,bytes,uint256,string)](node_modules/@openzeppelin/contracts/utils/Address.sol#L128-L137) has external calls inside a loop: [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)

node_modules/@openzeppelin/contracts/utils/Address.sol#L128-L137


## reentrancy-benign
Impact: Low
Confidence: Medium
 - [ ] ID-5
Reentrancy in [AbstractSilicaV2_1.proxyDeposit(address,uint256)](contracts/AbstractSilicaV2_1.sol#L480-L486):
        External calls:
        - [mintAmount = _deposit(msg.sender,_to,totalSupply(),amountSpecified)](contracts/AbstractSilicaV2_1.sol#L484)
                - [returndata = address(token).functionCall(data,SafeERC20: low-level call failed)](node_modules/@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol#L122)
                - [SafeERC20.safeTransferFrom(IERC20(paymentToken),from,to,amount)](contracts/AbstractSilicaV2_1.sol#L519)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        External calls sending eth:
        - [mintAmount = _deposit(msg.sender,_to,totalSupply(),amountSpecified)](contracts/AbstractSilicaV2_1.sol#L484)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        State variables written after the call(s):
        - [_mint(_to,mintAmount)](contracts/AbstractSilicaV2_1.sol#L485)
                - [_balances[account] += amount](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L259)

contracts/AbstractSilicaV2_1.sol#L480-L486


 - [ ] ID-6
Reentrancy in [AbstractSilicaV2_1.deposit(uint256)](contracts/AbstractSilicaV2_1.sol#L468-L473):
        External calls:
        - [mintAmount = _deposit(msg.sender,msg.sender,totalSupply(),amountSpecified)](contracts/AbstractSilicaV2_1.sol#L471)
                - [returndata = address(token).functionCall(data,SafeERC20: low-level call failed)](node_modules/@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol#L122)
                - [SafeERC20.safeTransferFrom(IERC20(paymentToken),from,to,amount)](contracts/AbstractSilicaV2_1.sol#L519)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        External calls sending eth:
        - [mintAmount = _deposit(msg.sender,msg.sender,totalSupply(),amountSpecified)](contracts/AbstractSilicaV2_1.sol#L471)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        State variables written after the call(s):
        - [_mint(msg.sender,mintAmount)](contracts/AbstractSilicaV2_1.sol#L472)
                - [_balances[account] += amount](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L259)

contracts/AbstractSilicaV2_1.sol#L468-L473


## reentrancy-events
Impact: Low
Confidence: Medium
 - [ ] ID-7
Reentrancy in [RewardsProxy.streamRewards(IRewardsProxy.StreamRequest[])](contracts/RewardsProxy.sol#L23-L31):
        External calls:
        - [_streamReward(streamRequests[i])](contracts/RewardsProxy.sol#L25)
                - [SafeERC20.safeTransferFrom(IERC20(streamRequest.rToken),msg.sender,streamRequest.silicaAddress,streamRequest.amount)](contracts/RewardsProxy.sol#L35)
                - [returndata = address(token).functionCall(data,SafeERC20: low-level call failed)](node_modules/@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol#L122)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        External calls sending eth:
        - [_streamReward(streamRequests[i])](contracts/RewardsProxy.sol#L25)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        Event emitted after the call(s):
        - [RewardsStreamed(streamRequests)](contracts/RewardsProxy.sol#L30)

contracts/RewardsProxy.sol#L23-L31


 - [ ] ID-8
Reentrancy in [SwapProxy.fillBuyOrder(OrderLib.BuyOrder,bytes,uint256,uint256)](contracts/SwapProxy.sol#L39-L107):
        External calls:
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateEthStakingSilicaV2_1(buyerOrder.rewardToken,buyerOrder.paymentToken,purchaseAmount,buyerOrder.endDay,buyerOrder.unitPrice,sellerAddress,additionalCollateralPercent)](contracts/SwapProxy.sol#L61-L69)
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateSilicaV2_1(buyerOrder.rewardToken,buyerOrder.paymentToken,purchaseAmount,buyerOrder.endDay,buyerOrder.unitPrice,sellerAddress,additionalCollateralPercent)](contracts/SwapProxy.sol#L71-L79)
        Event emitted after the call(s):
        - [BuyOrderFilled(silicaAddress,buyerOrderDigest,buyerOrder.signerAddress,sellerAddress,purchaseAmount)](contracts/SwapProxy.sol#L90)
        - [BuyOrderFilled(silicaAddress,buyerOrderDigest,buyerOrder.vaultAddress,sellerAddress,purchaseAmount)](contracts/SwapProxy.sol#L101)

contracts/SwapProxy.sol#L39-L107


 - [ ] ID-9
Reentrancy in [AbstractSilicaV2_1.proxyDeposit(address,uint256)](contracts/AbstractSilicaV2_1.sol#L480-L486):
        External calls:
        - [mintAmount = _deposit(msg.sender,_to,totalSupply(),amountSpecified)](contracts/AbstractSilicaV2_1.sol#L484)
                - [returndata = address(token).functionCall(data,SafeERC20: low-level call failed)](node_modules/@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol#L122)
                - [SafeERC20.safeTransferFrom(IERC20(paymentToken),from,to,amount)](contracts/AbstractSilicaV2_1.sol#L519)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        External calls sending eth:
        - [mintAmount = _deposit(msg.sender,_to,totalSupply(),amountSpecified)](contracts/AbstractSilicaV2_1.sol#L484)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        Event emitted after the call(s):
        - [Transfer(address(0),account,amount)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L261)
                - [_mint(_to,mintAmount)](contracts/AbstractSilicaV2_1.sol#L485)

contracts/AbstractSilicaV2_1.sol#L480-L486


 - [ ] ID-10
Reentrancy in [SwapProxy.routeBuy(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L109-L167):
        External calls:
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateEthStakingSilicaV2_1(sellerOrder.rewardToken,sellerOrder.paymentToken,sellerOrder.resourceAmount,sellerOrder.endDay,sellerOrder.unitPrice,sellerOrder.signerAddress,sellerOrder.additionalCollateralPercent)](contracts/SwapProxy.sol#L128-L136)
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateSilicaV2_1(sellerOrder.rewardToken,sellerOrder.paymentToken,sellerOrder.resourceAmount,sellerOrder.endDay,sellerOrder.unitPrice,sellerOrder.signerAddress,sellerOrder.additionalCollateralPercent)](contracts/SwapProxy.sol#L138-L146)
        Event emitted after the call(s):
        - [SellOrderFilled(silicaAddress,sellerOrderDigest,sellerOrder.signerAddress,buyerAddress,amount)](contracts/SwapProxy.sol#L152)

contracts/SwapProxy.sol#L109-L167


 - [ ] ID-11
Reentrancy in [SwapProxy.fillSellOrder(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L169-L227):
        External calls:
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateEthStakingSilicaV2_1(sellerOrder.rewardToken,sellerOrder.paymentToken,sellerOrder.resourceAmount,sellerOrder.endDay,sellerOrder.unitPrice,sellerOrder.signerAddress,sellerOrder.additionalCollateralPercent)](contracts/SwapProxy.sol#L189-L197)
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateSilicaV2_1(sellerOrder.rewardToken,sellerOrder.paymentToken,sellerOrder.resourceAmount,sellerOrder.endDay,sellerOrder.unitPrice,sellerOrder.signerAddress,sellerOrder.additionalCollateralPercent)](contracts/SwapProxy.sol#L199-L207)
        Event emitted after the call(s):
        - [SellOrderFilled(silicaAddress,sellerOrderDigest,sellerOrder.signerAddress,buyerAddress,amount)](contracts/SwapProxy.sol#L215)

contracts/SwapProxy.sol#L169-L227


 - [ ] ID-12
Reentrancy in [AbstractSilicaV2_1.sellerCollectPayoutExpired()](contracts/AbstractSilicaV2_1.sol#L651-L656):
        External calls:
        - [_transferRewardToSeller(rewardTokenPayout)](contracts/AbstractSilicaV2_1.sol#L654)
                - [SafeERC20.safeTransfer(IERC20(rewardToken),owner,amount)](contracts/AbstractSilicaV2_1.sol#L665)
                - [returndata = address(token).functionCall(data,SafeERC20: low-level call failed)](node_modules/@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol#L122)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        External calls sending eth:
        - [_transferRewardToSeller(rewardTokenPayout)](contracts/AbstractSilicaV2_1.sol#L654)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        Event emitted after the call(s):
        - [SellerCollectPayout(0,rewardTokenPayout)](contracts/AbstractSilicaV2_1.sol#L655)

contracts/AbstractSilicaV2_1.sol#L651-L656


 - [ ] ID-13
Reentrancy in [SilicaFactory._createSilicaV2_1(address,address,uint256,uint256,uint256,address,uint256)](contracts/SilicaFactory.sol#L215-L253):
        External calls:
        - [newSilicaV2.initialize(initializeData)](contracts/SilicaFactory.sol#L247)
        - [SafeERC20.safeTransferFrom(IERC20(_rewardTokenAddress),_sellerAddress,newContractAddress,collateralAmount)](contracts/SilicaFactory.sol#L248)
        Event emitted after the call(s):
        - [NewSilicaContract(newContractAddress,initializeData,uint16(MINING_SWAP_COMMODITY_TYPE))](contracts/SilicaFactory.sol#L250)

contracts/SilicaFactory.sol#L215-L253


 - [ ] ID-14
Reentrancy in [stdStorage.find(StdStorage)](lib/forge-std/src/Test.sol#L564-L659):
        External calls:
        - [vm_std_store.record()](lib/forge-std/src/Test.sol#L576)
        - [(reads) = vm_std_store.accesses(address(who))](lib/forge-std/src/Test.sol#L583)
        - [curr = vm_std_store.load(who,reads[0])](lib/forge-std/src/Test.sol#L585)
        Event emitted after the call(s):
        - [SlotFound(who,fsig,keccak256(bytes)(abi.encodePacked(ins,field_depth)),uint256(reads[0]))](lib/forge-std/src/Test.sol#L595-L600)
        - [WARNING_UninitedSlot(who,uint256(reads[0]))](lib/forge-std/src/Test.sol#L587)

lib/forge-std/src/Test.sol#L564-L659


 - [ ] ID-15
Reentrancy in [AbstractSilicaV2_1.deposit(uint256)](contracts/AbstractSilicaV2_1.sol#L468-L473):
        External calls:
        - [mintAmount = _deposit(msg.sender,msg.sender,totalSupply(),amountSpecified)](contracts/AbstractSilicaV2_1.sol#L471)
                - [returndata = address(token).functionCall(data,SafeERC20: low-level call failed)](node_modules/@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol#L122)
                - [SafeERC20.safeTransferFrom(IERC20(paymentToken),from,to,amount)](contracts/AbstractSilicaV2_1.sol#L519)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        External calls sending eth:
        - [mintAmount = _deposit(msg.sender,msg.sender,totalSupply(),amountSpecified)](contracts/AbstractSilicaV2_1.sol#L471)
                - [(success,returndata) = target.call{value: value}(data)](node_modules/@openzeppelin/contracts/utils/Address.sol#L135)
        Event emitted after the call(s):
        - [Transfer(address(0),account,amount)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L261)
                - [_mint(msg.sender,mintAmount)](contracts/AbstractSilicaV2_1.sol#L472)

contracts/AbstractSilicaV2_1.sol#L468-L473


 - [ ] ID-16
Reentrancy in [stdStorage.find(StdStorage)](lib/forge-std/src/Test.sol#L564-L659):
        External calls:
        - [vm_std_store.record()](lib/forge-std/src/Test.sol#L576)
        - [(reads) = vm_std_store.accesses(address(who))](lib/forge-std/src/Test.sol#L583)
        - [prev = vm_std_store.load(who,reads[i])](lib/forge-std/src/Test.sol#L609)
        - [vm_std_store.store(who,reads[i],bytes32(7))](lib/forge-std/src/Test.sol#L614)
        - [vm_std_store.store(who,reads[i],prev)](lib/forge-std/src/Test.sol#L639)
        Event emitted after the call(s):
        - [SlotFound(who,fsig,keccak256(bytes)(abi.encodePacked(ins,field_depth)),uint256(reads[i]))](lib/forge-std/src/Test.sol#L624-L629)
        - [WARNING_UninitedSlot(who,uint256(reads[i]))](lib/forge-std/src/Test.sol#L611)

lib/forge-std/src/Test.sol#L564-L659


 - [ ] ID-17
Reentrancy in [SilicaFactory._createEthStakingSilicaV2_1(address,address,uint256,uint256,uint256,address,uint256)](contracts/SilicaFactory.sol#L311-L348):
        External calls:
        - [newSilicaV2.initialize(initializeData)](contracts/SilicaFactory.sol#L342)
        - [SafeERC20.safeTransferFrom(IERC20(_rewardTokenAddress),_sellerAddress,newContractAddress,collateralAmount)](contracts/SilicaFactory.sol#L343)
        Event emitted after the call(s):
        - [NewSilicaContract(newContractAddress,initializeData,uint16(ETH_STAKING_COMMODITY_TYPE))](contracts/SilicaFactory.sol#L345)

contracts/SilicaFactory.sol#L311-L348


## timestamp
Impact: Low
Confidence: Medium
 - [ ] ID-18
[SwapProxy.routeBuy(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L109-L167) uses timestamp for comparisons
        Dangerous comparisons:
        - [require(bool,string)(block.timestamp <= sellerOrder.orderExpirationTimestamp,order expired)](contracts/SwapProxy.sol#L125)

contracts/SwapProxy.sol#L109-L167


 - [ ] ID-19
[SwapProxy.fillBuyOrder(OrderLib.BuyOrder,bytes,uint256,uint256)](contracts/SwapProxy.sol#L39-L107) uses timestamp for comparisons
        Dangerous comparisons:
        - [require(bool,string)(block.timestamp <= buyerOrder.orderExpirationTimestamp,order expired)](contracts/SwapProxy.sol#L52)

contracts/SwapProxy.sol#L39-L107


 - [ ] ID-20
[SwapProxy.fillSellOrder(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L169-L227) uses timestamp for comparisons
        Dangerous comparisons:
        - [require(bool,string)(block.timestamp <= sellerOrder.orderExpirationTimestamp,order expired)](contracts/SwapProxy.sol#L180)

contracts/SwapProxy.sol#L169-L227


## constable-states
Impact: Optimization
Confidence: High
 - [ ] ID-21
[SilicaV2_1Storage.silicaFactory](contracts/storage/SilicaV2_1Storage.sol#L12) should be constant 

contracts/storage/SilicaV2_1Storage.sol#L12