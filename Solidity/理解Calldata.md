## Calldata

Calldata是我们发送给函数的编码参数，在这里是发送给以太坊虚拟机（EVM）上的智能合约。每块calldata有32个字节长（或64个字符）。有两种类型的calldata：静态和动态。

静态变量是相当简单易懂的。另一方面，动态变量则要复杂得多，这可能是你难以直观地阅读原始calldata的原因。然而，一旦我们了解了动态变量是如何工作的，你就能轻松地阅读原始calldata了。

## 编码Calldata

要对类型进行编码，你可以将它们传入abi.encode(parameters)方法，以生成原始calldata。

如果你想为一个特定的接口函数编码Calldata，你可以使用 abi.encodeWithSelector(selector, parameters)。这将与直接传入函数和它的参数一样。

```
interface A {
  function transfer(uint256[] memory ids, address to) virtual external;
}

contract B {
  function a(uint256[] memory ids, address to) external pure returns(bytes memory) {
    return abi.encodeWithSelector(A.transfer.selector, ids, to);
  }
}
```
方法.selector产生了4个字节（称为：函数选择器），在接口上代表该方法。我们用它来告诉EVM，我们正在向该函数发送我们的calldata。这就是UniswapV2如何实现闪电兑换。

还有abi.encodePacked(...)，它可以有效地将所有动态变量放在一起，去掉0的填充。它的问题是，它不能防止碰撞，只有在你确定了参数的类型和长度时才可以使用。

## 解码Calldata

如果calldata是用abi.encode(...)创建的，那么我们可以用abi.decode(...)对参数进行解码，只要传入我们想把calldata解码成的参数。

```
(uint256 a, uint256 b) = abi.decode(data, (uint256, uint256))
```

## 静态变量

静态变量是以下类型的简单编码表示，uint , int, address, bool, bytes1 to bytes32 (包括函数选择器), 和tuple (然而它们可以有动态变量)。
```
pragma solidity 0.8.17;
contract Example {
    function transfer(uint256 amount, address to) external;
}

//输入参数
//amount: 1300655506
//address: 0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45

//生成的calldata
0x000000000000000000000000000000000000000000000000000000004d866d9200000000000000000000000068b3465833fb72a70ecdf485e0e4c7bd8665fc45

解析：去掉0x,然后把每一行分成64个字符(32字节)

0x 
//uint256
000000000000000000000000000000000000000000000000000000004d866d92
//address
00000000000000000000000068b3465833fb72a70ecdf485e0e4c7bd8665fc45

```

## 函数

我们需要知道参数类型的顺序，并使用一种叫做 "keccak256 "的Hash 算法，将输入的数据变成一个32字节的 hash 值：

```
function transfer(uint256 amount, address to) external;

获取方法哈希值

keccak256("transfer(uint256,address)");

将返回0xb7760c8fd605b6ef5a068e1720c115665f9699a5c439e3c0ee9709290ff8a3bb

函数签名取4个字节（8个字符） b7760c8f

```

## 动态变量







