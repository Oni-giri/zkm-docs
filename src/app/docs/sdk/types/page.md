---
title: Types
---

## Barretenberg
### AssetValue
```ts
interface AssetValue {
    assetId: number;
    value: bigint;
}
```

### BridgeCallData
```ts
import { BitConfig } from './bit_config';
export declare class BridgeCallData {
    readonly bridgeAddressId: number;
    readonly inputAssetIdA: number;
    readonly outputAssetIdA: number;
    readonly inputAssetIdB?: number | undefined;
    readonly outputAssetIdB?: number | undefined;
    readonly auxData: number;
    static ZERO: BridgeCallData;
    static ENCODED_LENGTH_IN_BYTES: number;
    readonly bitConfig: BitConfig;
    constructor(bridgeAddressId: number, inputAssetIdA: number, outputAssetIdA: number, inputAssetIdB?: number | undefined, outputAssetIdB?: number | undefined, auxData?: number);
    static random(): BridgeCallData;
    static fromBigInt(val: bigint): BridgeCallData;
    static fromBuffer(buf: Buffer): BridgeCallData;
    static fromString(str: string): BridgeCallData;
    get firstInputVirtual(): boolean;
    get secondInputVirtual(): boolean;
    get firstOutputVirtual(): boolean;
    get secondOutputVirtual(): boolean;
    get secondInputInUse(): boolean;
    get secondOutputInUse(): boolean;
    get numInputAssets(): 1 | 2;
    get numOutputAssets(): 1 | 2;
    toBigInt(): bigint;
    toBuffer(): Buffer;
    toString(): string;
    equals(id: BridgeCallData): boolean;
}
```

### DefiSettlementTime
```ts
enum DefiSettlementTime {
    DEADLINE = 0,
    NEXT_ROLLUP = 1,
    INSTANT = 2
}
```

### EthAddress
```ts
export interface EthAddress {
    isZero(): boolean;
    equals(rhs: EthAddress): boolean;
    toString(): string;
    toBuffer(): Buffer;
    toBuffer32(): Buffer;
}
export declare class EthAddress {
    private buffer;
    static ZERO: EthAddress;
    constructor(buffer: Buffer);
    static fromString(address: string): EthAddress;
    static random(): EthAddress;
    static isAddress(address: string): boolean;
    static checkAddressChecksum(address: string): boolean;
    static toChecksumAddress(address: string): string;
}
```

### EthereumProvider
```ts
interface EthereumProvider {
    request(args: RequestArguments): Promise<any>;
    on(notification: 'connect', listener: (connectInfo: ProviderConnectInfo) => void): this;
    on(notification: 'disconnect', listener: (error: ProviderRpcError) => void): this;
    on(notification: 'chainChanged', listener: (chainId: string) => void): this;
    on(notification: 'accountsChanged', listener: (accounts: string[]) => void): this;
    on(notification: 'message', listener: (message: ProviderMessage) => void): this;
    removeListener(notification: 'connect', listener: (connectInfo: ProviderConnectInfo) => void): this;
    removeListener(notification: 'disconnect', listener: (error: ProviderRpcError) => void): this;
    removeListener(notification: 'chainChanged', listener: (chainId: string) => void): this;
    removeListener(notification: 'accountsChanged', listener: (accounts: string[]) => void): this;
    removeListener(notification: 'message', listener: (message: ProviderMessage) => void): this;
}
```

### GrumpkinAddress
```ts
class GrumpkinAddress {
    private buffer;
    static SIZE: number;
    static ZERO: GrumpkinAddress;
    constructor(buffer: Buffer);
    static isAddress(address: string): boolean;
    static fromString(address: string): GrumpkinAddress;
    /**
     * NOT a valid address! Do not use in proofs.
     */
    static random(): GrumpkinAddress;
    /**
     * A valid address (is a point on the curve).
     */
    static one(): GrumpkinAddress;
    equals(rhs: GrumpkinAddress): boolean;
    toBuffer(): Buffer;
    x(): Buffer;
    y(): Buffer;
    toString(): string;
    toShortString(): string;
}
```

### BlockSource
```ts
interface RollupProvider extends BlockSource {
    sendTxs(txs: Tx[]): Promise<TxId[]>;
    getStatus(): Promise<RollupProviderStatus>;
    getTxFees(assetId: number): Promise<AssetValue[][]>;
    getDefiFees(bridgeId: BridgeId): Promise<AssetValue[]>;
    getPendingTxs(): Promise<PendingTx[]>;
    getPendingNoteNullifiers(): Promise<Buffer[]>;
    getPendingDepositTxs(): Promise<DepositTx[]>;
    clientLog(msg: any): Promise<void>;
    getInitialWorldState(): Promise<InitialWorldState>;
    isAccountRegistered(accountPublicKey: GrumpkinAddress): Promise<boolean>;
    isAliasRegistered(alias: string): Promise<boolean>;
    isAliasRegisteredToAccount(accountPublicKey: GrumpkinAddress, alias: string): Promise<boolean>;
}
```

### SchnorrSignature
```ts
class SchnorrSignature {
    private buffer;
    static SIZE: number;
    constructor(buffer: Buffer);
    static isSignature(signature: string): boolean;
    static fromString(signature: string): SchnorrSignature;
    static randomSignature(): SchnorrSignature;
    s(): Buffer;
    e(): Buffer;
    toBuffer(): Buffer;
    toString(): string;
}
```

### TxId
```ts
class TxId {
    private buffer;
    constructor(buffer: Buffer);
    static deserialize(buffer: Buffer, offset: number): {
        elem: TxId;
        adv: number;
    };
    static fromString(hash: string): TxId;
    static random(): TxId;
    equals(rhs: TxId): boolean;
    toBuffer(): Buffer;
    toString(): string;
    toDepositSigningData(): Buffer;
}
```

### TxSettlementTime
```ts
enum TxSettlementTime {
    NEXT_ROLLUP = 0,
    INSTANT = 1
}
```

## Bridge client

### AssetValue
```ts
export interface AssetValue {
    assetId: bigint;
    amount: bigint;
}
```
### AztecAssetType
```ts
export declare enum AztecAssetType {
    ETH = 0,
    ERC20 = 1,
    VIRTUAL = 2,
    NOT_USED = 3
}
```

### SolidityType
```ts
export declare enum SolidityType {
    uint8 = 0,
    uint16 = 1,
    uint24 = 2,
    uint32 = 3,
    uint64 = 4,
    boolean = 5,
    string = 6,
    bytes = 7
}
```

### AztecAsset
```ts
export interface AztecAsset {
    id: bigint;
    assetType: AztecAssetType;
    erc20Address: string;
}
```

### AuxDataConfig
```ts
export interface AuxDataConfig {
    start: number;
    length: number;
    description: string;
    solidityType: SolidityType;
}
````

### BridgeDataFieldGetters
```ts
export interface BridgeDataFieldGetters {
    getInteractionPresentValue?(interactionNonce: bigint): Promise<AssetValue[]>;
    getAuxData?(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset): Promise<bigint[]>;
    auxDataConfig: AuxDataConfig[];
    getExpectedOutput(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset, auxData: bigint, inputValue: bigint): Promise<bigint[]>;
    getExpiration?(interactionNonce: bigint): Promise<bigint>;
    hasFinalised?(interactionNonce: bigint): Promise<Boolean>;
    getExpectedYield?(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset, auxData: bigint, inputValue: bigint): Promise<number[]>;
    getMarketSize?(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset, auxData: bigint): Promise<AssetValue[]>;
    getCurrentYield?(interactionNonce: bigint): Promise<number[]>;
    lPAuxData?(data: bigint[]): Promise<bigint[]>;
}
```

### BatchSwapStep
```ts
export declare type BatchSwapStep = {
    poolId: string;
    assetInIndex: number;
    assetOutIndex: number;
    amount: string;
    userData: string;
};
```
### SwapType
```ts
export declare enum SwapType {
    SwapExactIn = 0,
    SwapExactOut = 1
}
```

### FundManagement
```ts
export declare type FundManagement = {
    sender: string;
    recipient: string;
    fromInternalBalance: boolean;
    toInternalBalance: boolean;
};
```
### ChainProperties
```ts
export declare type ChainProperties = {
    eventBatchSize: number;
};
````
### ElementBridgeData
```ts
export declare class ElementBridgeData implements BridgeDataFieldGetters {
    private elementBridgeContract;
    private balancerContract;
    private rollupContract;
    private chainProperties;
    scalingFactor: bigint;
    private interactionBlockNumbers;
    private constructor();
    static create(provider: EthereumProvider, elementBridgeAddress: EthAddress, balancerAddress: EthAddress, rollupContractAddress: EthAddress, chainProperties?: ChainProperties): ElementBridgeData;
    private storeEventBlocks;
    private getCurrentBlock;
    private findDefiEventForNonce;
    getInteractionPresentValue(interactionNonce: bigint): Promise<AssetValue[]>;
    getCurrentYield(interactionNonce: bigint): Promise<number[]>;
    getAuxData(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset): Promise<bigint[]>;
    auxDataConfig: AuxDataConfig[];
    getExpectedOutput(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset, auxData: bigint, precision: bigint): Promise<bigint[]>;
    getExpectedYield(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset, auxData: bigint, precision: bigint): Promise<number[]>;
    getMarketSize(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset, auxData: bigint): Promise<AssetValue[]>;
    getExpiration(interactionNonce: bigint): Promise<bigint>;
    hasFinalised(interactionNonce: bigint): Promise<Boolean>;
}
```

### LidoBridgeData
```ts
import { AssetValue, AuxDataConfig, AztecAsset, BridgeDataFieldGetters } from "../bridge-data";
import { EthereumProvider } from "@aztec/barretenberg/blockchain";
import { EthAddress } from "@aztec/barretenberg/address";
export declare class LidoBridgeData implements BridgeDataFieldGetters {
    private wstETHContract;
    private lidoOracleContract;
    private curvePoolContract;
    scalingFactor: bigint;
    private constructor();
    static create(provider: EthereumProvider, wstEthAddress: EthAddress, lidoOracleAddress: EthAddress, curvePoolAddress: EthAddress): LidoBridgeData;
    auxDataConfig: AuxDataConfig[];
    getInteractionPresentValue(interactionNonce: bigint): Promise<AssetValue[]>;
    getAuxData(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset): Promise<bigint[]>;
    getExpectedOutput(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset, auxData: bigint, inputValue: bigint): Promise<bigint[]>;
    getExpectedYield(inputAssetA: AztecAsset, inputAssetB: AztecAsset, outputAssetA: AztecAsset, outputAssetB: AztecAsset, auxData: bigint, precision: bigint): Promise<number[]>;
    getMarketS
```

## SDK
### AddSpendingKeyController
```ts
export declare class AddSpendingKeyController {
    readonly userId: GrumpkinAddress;
    private readonly userSigner;
    readonly spendingPublicKey1: GrumpkinAddress;
    readonly spendingPublicKey2: GrumpkinAddress | undefined;
    readonly fee: AssetValue;
    private readonly core;
    private readonly requireFeePayingTx;
    private proofOutput?;
    private feeProofOutputs;
    private txIds;
    constructor(userId: GrumpkinAddress, userSigner: Signer, spendingPublicKey1: GrumpkinAddress, spendingPublicKey2: GrumpkinAddress | undefined, fee: AssetValue, core: CoreSdkInterface);
    createProof(): Promise<void>;
    exportProofTxs(): import("@aztec/barretenberg/rollup_provider").Tx[];
    send(): Promise<TxId>;
    awaitSettlement(timeout?: number): Promise<void>;
}
```

### AztecSDK
```ts
export declare class AztecSdk extends EventEmitter {
    private core;
    private blockchain;
    private provider;
    private feeCalculator;
    private txValueCalculator;
    constructor(core: CoreSdkInterface, blockchain: ClientEthereumBlockchain, provider: EthereumProvider);
    run(): Promise<void>;
    destroy(): Promise<void>;
    awaitSynchronised(timeout?: number): Promise<void>;
    isUserSynching(userId: GrumpkinAddress): Promise<boolean>;
    awaitUserSynchronised(userId: GrumpkinAddress, timeout?: number): Promise<void>;
    awaitSettlement(txId: TxId, timeout?: number): Promise<void>;
    awaitDefiDepositCompletion(txId: TxId, timeout?: number): Promise<void>;
    awaitDefiFinalisation(txId: TxId, timeout?: number): Promise<void>;
    awaitDefiSettlement(txId: TxId, timeout?: number): Promise<void>;
    awaitAllUserTxsSettled(timeout?: number): Promise<void>;
    awaitAllUserTxsClaimed(timeout?: number): Promise<void>;
    getLocalStatus(): Promise<import("../core_sdk").SdkStatus>;
    getRemoteStatus(): Promise<import("@aztec/barretenberg/rollup_provider").RollupProviderStatus>;
    isAccountRegistered(accountPublicKey: GrumpkinAddress, includePending?: boolean): Promise<boolean>;
    isAliasRegistered(alias: string, includePending?: boolean): Promise<boolean>;
    isAliasRegisteredToAccount(accountPublicKey: GrumpkinAddress, alias: string, includePending?: boolean): Promise<boolean>;
    getAccountPublicKey(alias: string): Promise<GrumpkinAddress | undefined>;
    getTxFees(assetId: number, { feeSignificantFigures }?: {
        feeSignificantFigures?: number | undefined;
    }): Promise<AssetValue[][]>;
    userExists(accountPublicKey: GrumpkinAddress): Promise<boolean>;
    addUser(accountPrivateKey: Buffer, noSync?: boolean): Promise<AztecSdkUser>;
    removeUser(userId: GrumpkinAddress): Promise<void>;
    /**
     * Returns a AztecSdkUser for a locally resolved user.
     */
    getUser(userId: GrumpkinAddress): Promise<AztecSdkUser>;
    getUserSyncedToRollup(userId: GrumpkinAddress): Promise<number>;
    getUsers(): Promise<GrumpkinAddress[]>;
    getAccountKeySigningData(): Buffer;
    getSpendingKeySigningData(): Buffer;
    generateAccountKeyPair(account: EthAddress, provider?: EthereumProvider): Promise<{
        publicKey: GrumpkinAddress;
        privateKey: Buffer;
    }>;
    generateSpendingKeyPair(account: EthAddress, provider?: EthereumProvider): Promise<{
        publicKey: GrumpkinAddress;
        privateKey: Buffer;
    }>;
    createSchnorrSigner(privateKey: Buffer): Promise<SchnorrSigner>;
    derivePublicKey(privateKey: Buffer): Promise<GrumpkinAddress>;
    getAssetIdByAddress(address: EthAddress, gasLimit?: number): number;
    getAssetIdBySymbol(symbol: string, gasLimit?: number): number;
    fromBaseUnits({ assetId, value }: AssetValue, symbol?: boolean, precision?: number): string;
    toBaseUnits(assetId: number, value: string): {
        assetId: number;
        value: bigint;
    };
    getAssetInfo(assetId: number): import("@aztec/barretenberg/blockchain").BlockchainAsset;
    isFeePayingAsset(assetId: number): Promise<boolean>;
    isVirtualAsset(assetId: number): boolean;
    mint({ assetId, value }: AssetValue, account: EthAddress, options?: SendTxOptions): Promise<TxHash>;
    setSupportedAsset(assetAddress: EthAddress, assetGasLimit?: number, options?: SendTxOptions): Promise<TxHash>;
    getBridgeAddressId(address: EthAddress, gasLimit?: number): number;
    setSupportedBridge(bridgeAddress: EthAddress, bridgeGasLimit?: number, options?: SendTxOptions): Promise<TxHash>;
    processAsyncDefiInteraction(interactionNonce: number, options?: SendTxOptions): Promise<TxHash>;
    getDepositFees(assetId: number, options?: {
        feeSignificantFigures?: number;
    }): Promise<AssetValue[]>;
    getPendingDepositTxs(): Promise<import("@aztec/barretenberg/rollup_provider").DepositTx[]>;
    createDepositController(depositor: EthAddress, assetValue: AssetValue, fee: AssetValue, recipient: GrumpkinAddress, recipientSpendingKeyRequired?: boolean, provider?: EthereumProvider): DepositController;
    getWithdrawFees(assetId: number, options?: GetFeesOptions & {
        recipient?: EthAddress;
        assetValue?: AssetValue;
    }): Promise<AssetValue[]>;
    getMaxWithdrawValue(userId: GrumpkinAddress, assetId: number, options?: GetMaxTxValueOptions & {
        recipient?: EthAddress;
    }): Promise<{
        assetId: number;
        value: bigint;
        fee: {
            assetId: number;
            value: bigint;
        };
    }>;
    createWithdrawController(userId: GrumpkinAddress, userSigner: Signer, assetValue: AssetValue, fee: AssetValue, to: EthAddress): WithdrawController;
    getTransferFees(assetId: number, options?: GetFeesOptions & {
        assetValue?: AssetValue;
    }): Promise<AssetValue[]>;
    getMaxTransferValue(userId: GrumpkinAddress, assetId: number, options?: GetMaxTxValueOptions): Promise<{
        assetId: number;
        value: bigint;
        fee: {
            assetId: number;
            value: bigint;
        };
    }>;
    createTransferController(userId: GrumpkinAddress, userSigner: Signer, assetValue: AssetValue, fee: AssetValue, recipient: GrumpkinAddress, recipientSpendingKeyRequired?: boolean): TransferController;
    getDefiFees(bridgeCallData: BridgeCallData, options?: GetFeesOptions & {
        assetValue?: AssetValue;
    }): Promise<AssetValue[]>;
    getMaxDefiValue(userId: GrumpkinAddress, bridgeCallData: BridgeCallData, options?: Omit<GetMaxTxValueOptions, 'txSettlementTime'> & {
        txSettlementTime?: DefiSettlementTime;
    }): Promise<{
        assetId: number;
        value: bigint;
        fee: {
            assetId: number;
            value: bigint;
        };
    }>;
    createDefiController(userId: GrumpkinAddress, userSigner: Signer, bridgeCallData: BridgeCallData, assetValue: AssetValue, fee: AssetValue): DefiController;
    generateAccountRecoveryData(accountPublicKey: GrumpkinAddress, alias: string, trustedThirdPartyPublicKeys: GrumpkinAddress[]): Promise<RecoveryPayload[]>;
    getRegisterFees(assetId: number, options?: {
        feeSignificantFigures?: number;
    }): Promise<AssetValue[]>;
    createRegisterController(userId: GrumpkinAddress, alias: string, accountPrivateKey: Buffer, spendingPublicKey: GrumpkinAddress, recoveryPublicKey: GrumpkinAddress | undefined, deposit: AssetValue, fee: AssetValue, depositor?: EthAddress, provider?: EthereumProvider): RegisterController;
    getRecoverAccountFees(assetId: number, options?: {
        feeSignificantFigures?: number;
    }): Promise<AssetValue[]>;
    createRecoverAccountController(recoveryPayload: RecoveryPayload, deposit: AssetValue, fee: AssetValue, depositor?: EthAddress, provider?: EthereumProvider): RecoverAccountController;
    getAddSpendingKeyFees(assetId: number, options?: {
        feeSignificantFigures?: number;
    }): Promise<{
        value: bigint;
        assetId: number;
    }[]>;
    createAddSpendingKeyController(userId: GrumpkinAddress, userSigner: Signer, spendingPublicKey1: GrumpkinAddress, spendingPublicKey2: GrumpkinAddress | undefined, fee: AssetValue): AddSpendingKeyController;
    getMigrateAccountFees(assetId: number, options?: {
        feeSignificantFigures?: number;
    }): Promise<{
        value: bigint;
        assetId: number;
    }[]>;
    createMigrateAccountController(userId: GrumpkinAddress, userSigner: Signer, newAccountPrivateKey: Buffer, newSpendingPublicKey: GrumpkinAddress, recoveryPublicKey: GrumpkinAddress | undefined, deposit: AssetValue, fee: AssetValue, depositor?: EthAddress, provider?: EthereumProvider): MigrateAccountController;
    getProofTxsFees(assetId: number, proofTxs: Tx[], options?: GetFeesOptions): Promise<{
        value: bigint;
        assetId: number;
    }[]>;
    createFeeController(userId: GrumpkinAddress, userSigner: Signer, proofTxs: Tx[], fee: AssetValue): FeeController;
    depositFundsToContract({ assetId, value }: AssetValue, from: EthAddress, provider?: EthereumProvider): Promise<TxHash>;
    getUserPendingDeposit(assetId: number, account: EthAddress): Promise<bigint>;
    getUserPendingFunds(assetId: number, account: EthAddress): Promise<bigint>;
    isContract(address: EthAddress): Promise<boolean>;
    validateSignature(publicOwner: EthAddress, signature: Buffer, signingData: Buffer): boolean;
    getTransactionReceipt(txHash: TxHash, timeout?: number, interval?: number): Promise<Receipt>;
    flushRollup(userId: GrumpkinAddress, userSigner: Signer): Promise<void>;
    getSpendingKeys(userId: GrumpkinAddress): Promise<Buffer[]>;
    getPublicBalance(ethAddress: EthAddress, assetId: number): Promise<{
        assetId: number;
        value: bigint;
    }>;
    getBalances(userId: GrumpkinAddress): Promise<AssetValue[]>;
    getBalance(userId: GrumpkinAddress, assetId: number): Promise<{
        assetId: number;
        value: bigint;
    }>;
    getFormattedBalance(userId: GrumpkinAddress, assetId: number, symbol?: boolean, precision?: number): Promise<string>;
    getSpendableSum(userId: GrumpkinAddress, assetId: number, spendingKeyRequired?: boolean, excludePendingNotes?: boolean): Promise<bigint>;
    getSpendableSums(userId: GrumpkinAddress, spendingKeyRequired?: boolean, excludePendingNotes?: boolean): Promise<AssetValue[]>;
    getMaxSpendableValue(userId: GrumpkinAddress, assetId: number, spendingKeyRequired?: boolean, excludePendingNotes?: boolean, numNotes?: number): Promise<bigint>;
    getUserTxs(userId: GrumpkinAddress): Promise<(UserAccountTx | UserDefiTx | import("../user_tx").UserDefiClaimTx | UserPaymentTx)[]>;
    getPaymentTxs(userId: GrumpkinAddress): Promise<UserPaymentTx[]>;
    getAccountTxs(userId: GrumpkinAddress): Promise<UserAccountTx[]>;
    getDefiTxs(userId: GrumpkinAddress): Promise<UserDefiTx[]>;
}
```

### AztecSDK User
```ts
export declare class AztecSdkUser {
    id: GrumpkinAddress;
    private sdk;
    constructor(id: GrumpkinAddress, sdk: AztecSdk);
    isSynching(): Promise<boolean>;
    awaitSynchronised(timeout?: number): Promise<void>;
    getSyncedToRollup(): Promise<number>;
    getSpendingKeys(): Promise<Buffer[]>;
    getBalance(assetId: number): Promise<{
        assetId: number;
        value: bigint;
    }>;
    getSpendableSum(assetId: number, spendingKeyRequired?: boolean, excludePendingNotes?: boolean): Promise<bigint>;
    getSpendableSums(spendingKeyRequired?: boolean, excludePendingNotes?: boolean): Promise<import("@aztec/barretenberg/asset").AssetValue[]>;
    getMaxSpendableValue(assetId: number, spendingKeyRequired?: boolean, excludePendingNotes?: boolean, numNotes?: number): Promise<bigint>;
    getTxs(): Promise<(import("..").UserAccountTx | import("..").UserDefiTx | import("..").UserDefiClaimTx | import("..").UserPaymentTx)[]>;
    getPaymentTxs(): Promise<import("..").UserPaymentTx[]>;
    getAccountTxs(): Promise<import("..").UserAccountTx[]>;
    getDefiTxs(): Promise<import("..").UserDefiTx[]>;
}
```


### DefiController
```ts
export declare class DefiController {
    readonly userId: GrumpkinAddress;
    private readonly userSigner;
    readonly bridgeCallData: BridgeCallData;
    readonly assetValue: AssetValue;
    readonly fee: AssetValue;
    private readonly core;
    private readonly requireFeePayingTx;
    private proofOutput?;
    private jsProofOutputs;
    private feeProofOutputs;
    private txIds;
    constructor(userId: GrumpkinAddress, userSigner: Signer, bridgeCallData: BridgeCallData, assetValue: AssetValue, fee: AssetValue, core: CoreSdkInterface);
    createProof(): Promise<void>;
    exportProofTxs(): import("@aztec/barretenberg/rollup_provider").Tx[];
    send(): Promise<TxId>;
    awaitDefiDepositCompletion(timeout?: number): Promise<void>;
    awaitDefiFinalisation(timeout?: number): Promise<void>;
    awaitSettlement(timeout?: number): Promise<void>;
    getInteractionNonce(): Promise<number | undefined>;
    private getDefiTxId;
}
```

### DepositController
```ts
export declare class DepositController extends DepositHandler {
    readonly assetValue: AssetValue;
    readonly fee: AssetValue;
    readonly depositor: EthAddress;
    readonly recipient: GrumpkinAddress;
    readonly recipientSpendingKeyRequired: boolean;
    protected readonly core: CoreSdkInterface;
    private txIds;
    constructor(assetValue: AssetValue, fee: AssetValue, depositor: EthAddress, recipient: GrumpkinAddress, recipientSpendingKeyRequired: boolean, core: CoreSdkInterface, blockchain: ClientEthereumBlockchain, provider: EthereumProvider);
    createProof(): Promise<void>;
    exportProofTxs(): import("@aztec/barretenberg/rollup_provider").Tx[];
    send(): Promise<TxId>;
    awaitSettlement(timeout?: number): Promise<void>;
}
```

### DepositHandler
```ts
export declare class DepositHandler {
    readonly assetValue: AssetValue;
    readonly fee: AssetValue;
    readonly depositor: EthAddress;
    readonly recipient: GrumpkinAddress;
    readonly recipientSpendingKeyRequired: boolean;
    protected readonly core: CoreSdkInterface;
    private readonly blockchain;
    private readonly provider;
    protected readonly publicInput: AssetValue;
    private depositProofOutput?;
    private pendingFundsStatus;
    constructor(assetValue: AssetValue, fee: AssetValue, depositor: EthAddress, recipient: GrumpkinAddress, recipientSpendingKeyRequired: boolean, core: CoreSdkInterface, blockchain: ClientEthereumBlockchain, provider: EthereumProvider);
    getPendingFunds(): Promise<bigint>;
    getRequiredFunds(): Promise<bigint>;
    getPublicAllowance(): Promise<bigint>;
    hasPermitSupport(): boolean;
    approve(permitDeadline?: bigint): Promise<TxHash | undefined>;
    awaitApprove(timeout?: number, interval?: number): Promise<void>;
    depositFundsToContract(permitDeadline?: bigint): Promise<TxHash | undefined>;
    depositFundsToContractWithNonStandardPermit(permitDeadline?: bigint): Promise<TxHash>;
    awaitDepositFundsToContract(timeout?: number, interval?: number): Promise<true | undefined>;
    createProof(txRefNo?: number): Promise<void>;
    getProofOutput(): ProofOutput;
    getProofHash(): Buffer;
    isProofApproved(): Promise<boolean>;
    approveProof(): Promise<TxHash | undefined>;
    awaitApproveProof(timeout?: number, interval?: number): Promise<true | undefined>;
    getSigningData(): Buffer;
    sign(): Promise<void>;
    isSignatureValid(): boolean;
    private getPendingFundsStatus;
    private createPermitArgs;
    private createPermitArgsNonStandard;
    private getContractChainId;
    private sendTransactionAndCheckOnchainData;
    private awaitTransaction;
}
```

### FeeController
```ts
export declare class FeeController {
    readonly userId: GrumpkinAddress;
    private readonly userSigner;
    readonly proofTxs: Tx[];
    readonly fee: AssetValue;
    private readonly core;
    private feeProofOutputs;
    private txIds;
    constructor(userId: GrumpkinAddress, userSigner: Signer, proofTxs: Tx[], fee: AssetValue, core: CoreSdkInterface);
    createProof(): Promise<void>;
    send(): Promise<TxId>;
    awaitSettlement(timeout?: number): Promise<void>;
}
```

### MigrateAccountController
```ts
export declare class MigrateAccountController extends DepositHandler {
    readonly userId: GrumpkinAddress;
    private readonly userSigner;
    readonly newAccountPrivateKey: Buffer;
    readonly newSpendingPublicKey: GrumpkinAddress;
    readonly recoveryPublicKey: GrumpkinAddress | undefined;
    readonly deposit: AssetValue;
    readonly fee: AssetValue;
    readonly depositor: EthAddress;
    protected readonly core: CoreSdkInterface;
    private proofOutput?;
    private txIds;
    private requireDeposit;
    constructor(userId: GrumpkinAddress, userSigner: Signer, newAccountPrivateKey: Buffer, newSpendingPublicKey: GrumpkinAddress, recoveryPublicKey: GrumpkinAddress | undefined, deposit: AssetValue, fee: AssetValue, depositor: EthAddress, core: CoreSdkInterface, blockchain: ClientEthereumBlockchain, provider: EthereumProvider);
    createProof(): Promise<void>;
    exportProofTxs(): import("@aztec/barretenberg/rollup_provider").Tx[];
    send(): Promise<TxId>;
    awaitSettlement(timeout?: number): Promise<void>;
    private getProofOutputs;
}
```

### RecoveryAccountController
```ts
export declare class RecoverAccountController extends DepositHandler {
    readonly recoveryPayload: RecoveryPayload;
    readonly deposit: AssetValue;
    readonly fee: AssetValue;
    readonly depositor: EthAddress;
    protected readonly core: CoreSdkInterface;
    private proofOutput;
    private txIds;
    private requireDeposit;
    constructor(recoveryPayload: RecoveryPayload, deposit: AssetValue, fee: AssetValue, depositor: EthAddress, core: CoreSdkInterface, blockchain: ClientEthereumBlockchain, provider: EthereumProvider);
    createProof(): Promise<void>;
    exportProofTxs(): import("@aztec/barretenberg/rollup_provider").Tx[];
    send(): Promise<TxId>;
    awaitSettlement(timeout?: number): Promise<void>;
    private getProofOutputs;
}
```

### RecoveryData
```ts
export declare class RecoveryData {
    accountPublicKey: GrumpkinAddress;
    signature: SchnorrSignature;
    constructor(accountPublicKey: GrumpkinAddress, signature: SchnorrSignature);
    static fromBuffer(data: Buffer): RecoveryData;
    static fromString(data: string): RecoveryData;
    toBuffer(): Buffer;
    toString(): string;
}
```

### RecoverPayload
```ts
class RecoveryPayload {
    trustedThirdPartyPublicKey: GrumpkinAddress;
    recoveryPublicKey: GrumpkinAddress;
    recoveryData: RecoveryData;
    constructor(trustedThirdPartyPublicKey: GrumpkinAddress, recoveryPublicKey: GrumpkinAddress, recoveryData: RecoveryData);
    static fromBuffer(data: Buffer): RecoveryPayload;
    static fromString(data: string): RecoveryPayload;
    toBuffer(): Buffer;
    toString(): string;
}
```

### RegisterController
```ts
class RegisterController extends DepositHandler {
    readonly userId: GrumpkinAddress;
    readonly alias: string;
    private readonly accountPrivateKey;
    readonly spendingPublicKey: GrumpkinAddress;
    readonly recoveryPublicKey: GrumpkinAddress | undefined;
    readonly deposit: AssetValue;
    readonly fee: AssetValue;
    readonly depositor: EthAddress;
    protected readonly core: CoreSdkInterface;
    private proofOutput?;
    private txIds;
    private requireDeposit;
    constructor(userId: GrumpkinAddress, alias: string, accountPrivateKey: Buffer, spendingPublicKey: GrumpkinAddress, recoveryPublicKey: GrumpkinAddress | undefined, deposit: AssetValue, fee: AssetValue, depositor: EthAddress, core: CoreSdkInterface, blockchain: ClientEthereumBlockchain, provider: EthereumProvider);
    createProof(): Promise<void>;
    exportProofTxs(): import("@aztec/barretenberg/rollup_provider").Tx[];
    send(): Promise<TxId>;
    awaitSettlement(timeout?: number): Promise<void>;
    private getProofOutputs;
}
```


### SchnorrSigner
```ts
class SchnorrSigner implements Signer {
    private schnorr;
    private publicKey;
    private privateKey;
    constructor(schnorr: Schnorr, publicKey: GrumpkinAddress, privateKey: Buffer);
    getPublicKey(): GrumpkinAddress;
    signMessage(message: Buffer): Promise<SchnorrSignature>;
}
```

### Signer
```ts
interface Signer {
    getPublicKey(): GrumpkinAddress;
    signMessage(message: Buffer): Promise<SchnorrSignature>;
}
```

### TransferController
```ts
export declare class TransferController {
    readonly userId: GrumpkinAddress;
    private readonly userSigner;
    readonly assetValue: AssetValue;
    readonly fee: AssetValue;
    readonly recipient: GrumpkinAddress;
    readonly recipientSpendingKeyRequired: boolean;
    private readonly core;
    private readonly requireFeePayingTx;
    private proofOutputs;
    private feeProofOutputs;
    private txIds;
    constructor(userId: GrumpkinAddress, userSigner: Signer, assetValue: AssetValue, fee: AssetValue, recipient: GrumpkinAddress, recipientSpendingKeyRequired: boolean, core: CoreSdkInterface);
    createProof(): Promise<void>;
    exportProofTxs(): import("@aztec/barretenberg/rollup_provider").Tx[];
    send(): Promise<TxId>;
    awaitSettlement(timeout?: number): Promise<void>;
}
```

### WithdrawController
```ts
export declare class WithdrawController {
    readonly userId: GrumpkinAddress;
    private readonly userSigner;
    readonly assetValue: AssetValue;
    readonly fee: AssetValue;
    readonly recipient: EthAddress;
    private readonly core;
    private readonly requireFeePayingTx;
    private proofOutputs;
    private feeProofOutputs;
    private txIds;
    constructor(userId: GrumpkinAddress, userSigner: Signer, assetValue: AssetValue, fee: AssetValue, recipient: EthAddress, core: CoreSdkInterface);
    createProof(): Promise<void>;
    exportProofTxs(): import("@aztec/barretenberg/rollup_provider").Tx[];
    send(): Promise<TxId>;
    awaitSettlement(timeout?: number): Promise<void>;
}
```


