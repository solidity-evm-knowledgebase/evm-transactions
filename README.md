# evm-transactions

## Transaction Types

1. Transaction Type 0 (Legacy transactions)
  Transaction formation before the introduction of transaction types

2. Transaction Type 1 (0x01 transactions)
   0x1 - Optional Access Lists (EIP-2930):
    Same txs fields as legacy with additional accessList parameter

3. Transaction Type 2 (0x02 transactions)
  0x02 - EIP-1559 (Londong Fork):
  Aimed to tackle congestion and high fees - replaced gasPrice parameter with baseFee + added maxPriorityFeePerGas and maxFeePerGas (maxPriorityFeePerGas + baseFee)

4. Transaction Type 3 (0x03 transactions)
   Blob Transactions (EIP-4844) (Proto-Danksharding:
   Introduced in "Dencun" fork - introduced additional fiels: max_blob_fee_per_gas and blob_versioned_hashes

### Type 3 transactions: EIP-4844 (blobs) - Proto-Danksharding:

Blobs: Binary Large Objects

L2 Rollups compress multiple transactions (batch) into a transaction and send it to Ethereum L1.
The L1 then verifies that the batch of txs is good, but then doesn't care about the data anymore. (making it also expensive to store all this calldata).
Blobs transactions were created to mitigate this issue, where l2 can use these blobs to send data to eth l1, the l1 verifies this data and then it is deleted after 20-90 days

1. Blobs are a new transaction type that allows us to to store data on-chain for a short period of time.
2. We can't access the data itself, but we can access a hash of the data with the new BLOBHASH opcode. 
3. Blobs were added because rollups wanted a cheaper way to verify transactions.

#### How Rollups actually validate these transactions

1. rollup submits a transaction with a blob, along with some proof data.
2. rollup's contract on-chain access a hash of the blob with the 'BLOBHASH' opcode
3. It will then pass the blob-hash combined with the proof data to the new point evaluation opcode to help verify the transaction batch


### Additional Transaction Types on ZK-Sync:

5. 0x71: Typed Structured data (EIP-712):
   which is actually type 113 transactions; they enable features like native account abstraction on Zk-Sync
   Introduced additional fiels: gasPerPubData (max gas the sender is willing to pay for single byte of pub data (l2 state data submitted to l1)),  customSignature (if deployer not an eoa), paymasterParams and 
   factory_deps (contain bytecode of smart contract being deployed).

6. 0xff: Priority Transactions:
    enable us to send transactions from l1 directly to zksync l2.
