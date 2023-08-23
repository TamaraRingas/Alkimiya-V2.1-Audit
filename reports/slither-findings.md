Summary
 - [arbitrary-send-erc20](#arbitrary-send-erc20) (1 results) (High)
 - [reentrancy-eth](#reentrancy-eth) (2 results) (High)
 - [divide-before-multiply](#divide-before-multiply) (9 results) (Medium)
 - [reentrancy-no-eth](#reentrancy-no-eth) (3 results) (Medium)
 - [unchecked-lowlevel](#unchecked-lowlevel) (5 results) (Medium)
 - [uninitialized-local](#uninitialized-local) (7 results) (Medium)
 - [unused-return](#unused-return) (12 results) (Medium)
 - [constable-states](#constable-states) (2 results) (Optimization)
## arbitrary-send-erc20
Impact: High
Confidence: High
 - [ ] ID-0
[SwapProxy.fillBuyOrder(OrderLib.BuyOrder,bytes,uint256,uint256)](contracts/SwapProxy.sol#L39-L107) uses arbitrary from in transferFrom: [SafeERC20.safeTransferFrom(IERC20(buyerOrder.paymentToken),buyerOrder.signerAddress,address(this),reservedPrice)](contracts/SwapProxy.sol#L93)

contracts/SwapProxy.sol#L39-L107


## reentrancy-eth
Impact: High
Confidence: Medium
 - [ ] ID-1
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
                - [_totalSupply += amount](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L256)
        [ERC20._totalSupply](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L43) can be used in cross function reentrancies:
        - [ERC20._burn(address,uint256)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L277-L293)
        - [ERC20._mint(address,uint256)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L251-L264)
        - [ERC20.totalSupply()](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L94-L96)

contracts/AbstractSilicaV2_1.sol#L480-L486


 - [ ] ID-2
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
                - [_totalSupply += amount](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L256)
        [ERC20._totalSupply](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L43) can be used in cross function reentrancies:
        - [ERC20._burn(address,uint256)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L277-L293)
        - [ERC20._mint(address,uint256)](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L251-L264)
        - [ERC20.totalSupply()](node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L94-L96)

contracts/AbstractSilicaV2_1.sol#L468-L473


## divide-before-multiply
Impact: Medium
Confidence: Medium
 - [ ] ID-3
[Math.mulDiv(uint256,uint256,uint256)](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134) performs a multiplication on the result of a division:
        - [denominator = denominator / twos](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L101)
        - [inverse *= 2 - denominator * inverse](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L120)

node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134


 - [ ] ID-4
[Math.mulDiv(uint256,uint256,uint256)](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134) performs a multiplication on the result of a division:
        - [prod0 = prod0 / twos](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L104)
        - [result = prod0 * inverse](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L131)

node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134


 - [ ] ID-5
[Math.mulDiv(uint256,uint256,uint256)](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134) performs a multiplication on the result of a division:
        - [denominator = denominator / twos](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L101)
        - [inverse *= 2 - denominator * inverse](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L122)

node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134


 - [ ] ID-6
[Math.mulDiv(uint256,uint256,uint256)](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134) performs a multiplication on the result of a division:
        - [denominator = denominator / twos](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L101)
        - [inverse *= 2 - denominator * inverse](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L125)

node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134


 - [ ] ID-7
[PayoutMath._getHaircut(uint256,uint256)](contracts/libraries/math/PayoutMath.sol#L20-L25) performs a multiplication on the result of a division:
        - [multiplier = ((_numDepositsCompleted ** 3) * FIXED_POINT_SCALE_VALUE) / (contractNumberOfDepositsCubed)](contracts/libraries/math/PayoutMath.sol#L22)
        - [result = (HAIRCUT_BASE_PCT * multiplier) / (100 * FIXED_POINT_BASE)](contracts/libraries/math/PayoutMath.sol#L23)

contracts/libraries/math/PayoutMath.sol#L20-L25


 - [ ] ID-8
[Math.mulDiv(uint256,uint256,uint256)](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134) performs a multiplication on the result of a division:
        - [denominator = denominator / twos](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L101)
        - [inverse *= 2 - denominator * inverse](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L124)

node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134


 - [ ] ID-9
[Math.mulDiv(uint256,uint256,uint256)](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134) performs a multiplication on the result of a division:
        - [denominator = denominator / twos](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L101)
        - [inverse *= 2 - denominator * inverse](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L123)

node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134


 - [ ] ID-10
[Math.mulDiv(uint256,uint256,uint256)](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134) performs a multiplication on the result of a division:
        - [denominator = denominator / twos](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L101)
        - [inverse *= 2 - denominator * inverse](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L121)

node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134


 - [ ] ID-11
[Math.mulDiv(uint256,uint256,uint256)](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134) performs a multiplication on the result of a division:
        - [denominator = denominator / twos](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L101)
        - [inverse = (3 * denominator) ^ 2](node_modules/@openzeppelin/contracts/utils/math/Math.sol#L116)

node_modules/@openzeppelin/contracts/utils/math/Math.sol#L55-L134


## reentrancy-no-eth
Impact: Medium
Confidence: Medium
 - [ ] ID-12
Reentrancy in [SwapProxy.fillSellOrder(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L169-L227):
        External calls:
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateEthStakingSilicaV2_1(sellerOrder.rewardToken,sellerOrder.paymentToken,sellerOrder.resourceAmount,sellerOrder.endDay,sellerOrder.unitPrice,sellerOrder.signerAddress,sellerOrder.additionalCollateralPercent)](contracts/SwapProxy.sol#L189-L197)
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateSilicaV2_1(sellerOrder.rewardToken,sellerOrder.paymentToken,sellerOrder.resourceAmount,sellerOrder.endDay,sellerOrder.unitPrice,sellerOrder.signerAddress,sellerOrder.additionalCollateralPercent)](contracts/SwapProxy.sol#L199-L207)
        State variables written after the call(s):
        - [sellOrderToSilica[sellerOrderDigest] = silicaAddress](contracts/SwapProxy.sol#L212)
        [SwapProxy.sellOrderToSilica](contracts/SwapProxy.sol#L25) can be used in cross function reentrancies:
        - [SwapProxy.fillSellOrder(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L169-L227)
        - [SwapProxy.getSilicaAddressFromSellOrderHash(bytes32)](contracts/SwapProxy.sol#L263-L265)
        - [SwapProxy.routeBuy(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L109-L167)
        - [SwapProxy.sellOrderToSilica](contracts/SwapProxy.sol#L25)

contracts/SwapProxy.sol#L169-L227


 - [ ] ID-13
Reentrancy in [SwapProxy.routeBuy(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L109-L167):
        External calls:
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateEthStakingSilicaV2_1(sellerOrder.rewardToken,sellerOrder.paymentToken,sellerOrder.resourceAmount,sellerOrder.endDay,sellerOrder.unitPrice,sellerOrder.signerAddress,sellerOrder.additionalCollateralPercent)](contracts/SwapProxy.sol#L128-L136)
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateSilicaV2_1(sellerOrder.rewardToken,sellerOrder.paymentToken,sellerOrder.resourceAmount,sellerOrder.endDay,sellerOrder.unitPrice,sellerOrder.signerAddress,sellerOrder.additionalCollateralPercent)](contracts/SwapProxy.sol#L138-L146)
        State variables written after the call(s):
        - [sellOrderToSilica[sellerOrderDigest] = silicaAddress](contracts/SwapProxy.sol#L154)
        [SwapProxy.sellOrderToSilica](contracts/SwapProxy.sol#L25) can be used in cross function reentrancies:
        - [SwapProxy.fillSellOrder(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L169-L227)
        - [SwapProxy.getSilicaAddressFromSellOrderHash(bytes32)](contracts/SwapProxy.sol#L263-L265)
        - [SwapProxy.routeBuy(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L109-L167)
        - [SwapProxy.sellOrderToSilica](contracts/SwapProxy.sol#L25)

contracts/SwapProxy.sol#L109-L167


 - [ ] ID-14
Reentrancy in [SwapProxy.fillBuyOrder(OrderLib.BuyOrder,bytes,uint256,uint256)](contracts/SwapProxy.sol#L39-L107):
        External calls:
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateEthStakingSilicaV2_1(buyerOrder.rewardToken,buyerOrder.paymentToken,purchaseAmount,buyerOrder.endDay,buyerOrder.unitPrice,sellerAddress,additionalCollateralPercent)](contracts/SwapProxy.sol#L61-L69)
        - [silicaAddress = ISilicaFactory(silicaFactory).proxyCreateSilicaV2_1(buyerOrder.rewardToken,buyerOrder.paymentToken,purchaseAmount,buyerOrder.endDay,buyerOrder.unitPrice,sellerAddress,additionalCollateralPercent)](contracts/SwapProxy.sol#L71-L79)
        State variables written after the call(s):
        - [buyOrderToConsumedBudget[buyerOrderDigest] += purchaseAmount](contracts/SwapProxy.sol#L84)
        [SwapProxy.buyOrderToConsumedBudget](contracts/SwapProxy.sol#L22) can be used in cross function reentrancies:
        - [SwapProxy.buyOrderToConsumedBudget](contracts/SwapProxy.sol#L22)
        - [SwapProxy.fillBuyOrder(OrderLib.BuyOrder,bytes,uint256,uint256)](contracts/SwapProxy.sol#L39-L107)
        - [SwapProxy.getBudgetConsumedFromOrderHash(bytes32)](contracts/SwapProxy.sol#L258-L260)

contracts/SwapProxy.sol#L39-L107


## unchecked-lowlevel
Impact: Medium
Confidence: Medium
 - [ ] ID-15
[Test.deal(address,address,uint256,bool)](lib/forge-std/src/Test.sol#L141-L169) ignores return value by [(balData) = token.call(abi.encodeWithSelector(0x70a08231,to))](lib/forge-std/src/Test.sol#L148-L150)

lib/forge-std/src/Test.sol#L141-L169


 - [ ] ID-16
[Test.deal(address,address,uint256,bool)](lib/forge-std/src/Test.sol#L141-L169) ignores return value by [(totSupData) = token.call(abi.encodeWithSelector(0x18160ddd))](lib/forge-std/src/Test.sol#L158-L160)

lib/forge-std/src/Test.sol#L141-L169


 - [ ] ID-17
[stdStorage.find(StdStorage)](lib/forge-std/src/Test.sol#L564-L659) ignores return value by [(rdat) = who.staticcall(cald)](lib/forge-std/src/Test.sol#L579)

lib/forge-std/src/Test.sol#L564-L659


 - [ ] ID-18
[Address.sendValue(address,uint256)](node_modules/@openzeppelin/contracts/utils/Address.sol#L64-L69) ignores return value by [(success) = recipient.call{value: amount}()](node_modules/@openzeppelin/contracts/utils/Address.sol#L67)

node_modules/@openzeppelin/contracts/utils/Address.sol#L64-L69


 - [ ] ID-19
[stdStorage.checked_write(StdStorage,bytes32)](lib/forge-std/src/Test.sol#L734-L766) ignores return value by [(rdat) = who.staticcall(cald)](lib/forge-std/src/Test.sol#L750)

lib/forge-std/src/Test.sol#L734-L766


## uninitialized-local
Impact: Medium
Confidence: Medium
 - [ ] ID-20
[SilicaFactory._createSilicaV2_1(address,address,uint256,uint256,uint256,address,uint256).initializeData](contracts/SilicaFactory.sol#L236) is a local variable never initialized

contracts/SilicaFactory.sol#L236


 - [ ] ID-21
[SilicaFactory._getOracleData(address).oracleData](contracts/SilicaFactory.sol#L122) is a local variable never initialized

contracts/SilicaFactory.sol#L122


 - [ ] ID-22
[AbstractSilicaV2_1.getRewardDueNextOracleUpdate().balanceNeeded](contracts/AbstractSilicaV2_1.sol#L441) is a local variable never initialized

contracts/AbstractSilicaV2_1.sol#L441


 - [ ] ID-23
[SwapProxy.fillBuyOrder(OrderLib.BuyOrder,bytes,uint256,uint256).silicaAddress](contracts/SwapProxy.sol#L58) is a local variable never initialized

contracts/SwapProxy.sol#L58


 - [ ] ID-24
[SilicaFactory._createEthStakingSilicaV2_1(address,address,uint256,uint256,uint256,address,uint256).initializeData](contracts/SilicaFactory.sol#L331) is a local variable never initialized

contracts/SilicaFactory.sol#L331


 - [ ] ID-25
[SilicaFactory._getOracleEthStakingData(address).oracleData](contracts/SilicaFactory.sol#L134) is a local variable never initialized

contracts/SilicaFactory.sol#L134


 - [ ] ID-26
[SwapProxy.fillSellOrder(OrderLib.SellOrder,bytes,uint256).silicaAddress](contracts/SwapProxy.sol#L186) is a local variable never initialized

contracts/SwapProxy.sol#L186


## unused-return
Impact: Medium
Confidence: Medium
 - [ ] ID-27
[SwapProxy.fillBuyOrder(OrderLib.BuyOrder,bytes,uint256,uint256)](contracts/SwapProxy.sol#L39-L107) ignores return value by [IERC20(buyerOrder.paymentToken).approve(silicaAddress,reservedPrice)](contracts/SwapProxy.sol#L96)

contracts/SwapProxy.sol#L39-L107


 - [ ] ID-28
[SwapProxy.fillSellOrder(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L169-L227) ignores return value by [IERC20(sellerOrder.paymentToken).approve(silicaAddress,reservedPrice)](contracts/SwapProxy.sol#L223)

contracts/SwapProxy.sol#L169-L227


 - [ ] ID-29
[SilicaFactory._getOracleEthStakingData(address)](contracts/SilicaFactory.sol#L133-L144) ignores return value by [(baseRewardPerIncrementPerDay) = oracleEthStaking.get(lastIndexedDay)](contracts/SilicaFactory.sol#L139)

contracts/SilicaFactory.sol#L133-L144


 - [ ] ID-30
[stdStorage.find(StdStorage)](lib/forge-std/src/Test.sol#L564-L659) ignores return value by [(reads) = vm_std_store.accesses(address(who))](lib/forge-std/src/Test.sol#L583)

lib/forge-std/src/Test.sol#L564-L659


 - [ ] ID-31
[SilicaEthStaking._getRewardDueOnDay(uint256)](contracts/SilicaEthStaking.sol#L33-L40) ignores return value by [(baseRewardPerIncrementPerDay) = oracleEthStaking.get(_day)](contracts/SilicaEthStaking.sol#L37)

contracts/SilicaEthStaking.sol#L33-L40


 - [ ] ID-32
[SilicaV2_1._getRewardDueOnDay(uint256)](contracts/SilicaV2_1.sol#L31-L36) ignores return value by [(networkHashrate,networkReward) = oracle.get(_day)](contracts/SilicaV2_1.sol#L33)

contracts/SilicaV2_1.sol#L31-L36


 - [ ] ID-33
[SwapProxy.fillBuyOrder(OrderLib.BuyOrder,bytes,uint256,uint256)](contracts/SwapProxy.sol#L39-L107) ignores return value by [ISilicaVault(buyerOrder.vaultAddress).purchaseSilica(silicaAddress,reservedPrice)](contracts/SwapProxy.sol#L103)

contracts/SwapProxy.sol#L39-L107


 - [ ] ID-34
[SwapProxy.fillBuyOrder(OrderLib.BuyOrder,bytes,uint256,uint256)](contracts/SwapProxy.sol#L39-L107) ignores return value by [ISilicaV2_1(silicaAddress).proxyDeposit(buyerOrder.signerAddress,reservedPrice)](contracts/SwapProxy.sol#L99)

contracts/SwapProxy.sol#L39-L107


 - [ ] ID-35
[SilicaFactory._getOracleData(address)](contracts/SilicaFactory.sol#L121-L130) ignores return value by [(networkHashrate,networkReward) = oracle.get(lastIndexedDay)](contracts/SilicaFactory.sol#L125)

contracts/SilicaFactory.sol#L121-L130


 - [ ] ID-36
[SwapProxy.routeBuy(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L109-L167) ignores return value by [IERC20(sellerOrder.paymentToken).approve(silicaAddress,reservedPrice)](contracts/SwapProxy.sol#L163)

contracts/SwapProxy.sol#L109-L167


 - [ ] ID-37
[SwapProxy.fillSellOrder(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L169-L227) ignores return value by [ISilicaV2_1(silicaAddress).proxyDeposit(buyerAddress,reservedPrice)](contracts/SwapProxy.sol#L224)

contracts/SwapProxy.sol#L169-L227


 - [ ] ID-38
[SwapProxy.routeBuy(OrderLib.SellOrder,bytes,uint256)](contracts/SwapProxy.sol#L109-L167) ignores return value by [ISilicaV2_1(silicaAddress).proxyDeposit(buyerAddress,reservedPrice)](contracts/SwapProxy.sol#L164)

contracts/SwapProxy.sol#L109-L167


## constable-states
Impact: Optimization
Confidence: High
 - [ ] ID-39
[SilicaV2_1Storage.silicaFactory](contracts/storage/SilicaV2_1Storage.sol#L12) should be constant 

contracts/storage/SilicaV2_1Storage.sol#L12