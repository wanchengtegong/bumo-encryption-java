# BUMO JAVA ENCRYPTION使用文档

- [用途](#用途)
- [包引用](#包引用)
- [私钥](#私钥)
	- [构造对象](#构造对象)
	- [获取编码后私钥](#获取编码后私钥)
	- [获取编码后公钥](#获取编码后公钥)
	- [签名](#签名)
- [公钥](#公钥)
	- [构造对象](#构造对象)
	- [获取编码后地址](#获取编码后地址)
	- [验签](#验签)
- [密钥存储器](#密钥存储器)
	- [生成密钥存储器](#生成密钥存储器)
	- [解析密钥存储器](#解析密钥存储器)
- [助记词](#助记词)
	- [生成助记词](#生成助记词)
	- [根据助记词生成私钥](#根据助记词生成私钥)
- [计算哈希](#计算哈希)
- [举例说明](#举例说明)
	- [创建账户](#创建账户)
	- [发行资产](#发行资产)
	- [转移资产](#发行资产)
	- [转移bu资产](#转移bu资产)

## 用途
用于生成公私钥和地址，以及签名，和验签，只支持ED25519。

## 包引用
所依赖的jar包在jar文件夹中寻找，依赖的jar包如下：

1. bcprov-jdk15on-1.52.jar
2. eddsa-0.1.0.jar
3. fastjson-1.2.32.jar

## 私钥
### 构造对象
#### 签名方式构造

示例：
```java
PrivateKey privateKey = new PrivateKey(KeyType.ED25519);
````

### 私钥构造
参数是编码后的私钥

示例如下：
```java
String encPrivateKey = "privbsDGan4sA9ZYpEERhMe25k4K5tnJu1fNqfEHbyKfaV9XSYq7uMcy";
PrivateKey privateKey = new PrivateKey(encPrivateKey);
```

### 获取编码后私钥
#### 非静态接口
方法名：getEncPrivateKey
注意：调用此方法需要构造PrivateKey对象

请求参数：无

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| encPrivateKey | String | 编码后的私钥

例如：
```java
PrivateKey privateKey = new PrivateKey(KeyType.ED25519);
String encPrivateKey = privateKey.getEncPrivateKey();
```

### 获取编码后公钥
#### 非静态接口
方法名：getEncPublicKey
注意：调用此方法需要构造PrivateKey对象

请求参数：无

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| encPublicKey | String | 编码后的公钥

例如：
```java
PrivateKey privateKey = new PrivateKey(KeyType.ED25519);
String encPublicKey = privateKey.getEncPublicKey();
```

#### 静态接口
方法名：getEncPublicKey
注意：调用此方法不需要构造PrivateKey对象

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| EncPrivateKey | String | 编码的私钥

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| encPublicKey | String | 编码的公钥

例如：
```java
String encPrivateKey = "privbsDGan4sA9ZYpEERhMe25k4K5tnJu1fNqfEHbyKfaV9XSYq7uMcy";
String encPublicKey = PrivateKey.getEncPublicKey(encPrivateKey);
```

### 签名
#### 非静态接口
方法名: sign
注意：调用此方法需要构造PrivateKey对象

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| msg | byte[] | 待签名信息

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| signMsg | byte[] | 签名后信息

例如：
```java
PrivateKey privateKey = PrivateKey(KeyType.ED25519);
String src = "test";
byte[] signMsg = privateKey.sign(src.getBytes());
```

#### 静态接口
方法名: sign
注意：调用此方法不需要构造PrivateKey对象

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| msg | byte[] | 待签名信息
| privateKey | String | 私钥

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| signMsg | byte[] | 签名后信息

例如：
```java
String src = "test";
String privateKey = "privbsDGan4sA9ZYpEERhMe25k4K5tnJu1fNqfEHbyKfaV9XSYq7uMcy";
byte[] sign = PrivateKey.sign(src.getBytes(), privateKey);
```

## 公钥
### 构造对象
#### 签名方式构造
参数是编码的公钥

示例如下：
```java
String publicKey = "b0014085888f15e6fdae80827f5ec129f7e9323cf60732e7f8259fa2d68a282e8eed51fad13f";
PublicKey publicKey = new PublicKey(encPublicKey);
```

### 获取编码后地址
#### 非静态接口
方法名：getEncAddress
注意：调用此方法需要构造PublicKey对象

请求参数： 无

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| encAddress | String | 编码的地址

例如：
```java
String publicKey = "b0014085888f15e6fdae80827f5ec129f7e9323cf60732e7f8259fa2d68a282e8eed51fad13f";
PublicKey publicKey = new PublicKey(encPublicKey);
String encAddress = publicKey.getEncAddress();
```

#### 静态接口
方法名：getEncAddress
注意：调用此方法不需要构造PublicKey对象

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| encPrivateKey | String | 编码的私钥

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| encAddress | String | 编码后的地址

例如：
```java
String publicKey = "b0014085888f15e6fdae80827f5ec129f7e9323cf60732e7f8259fa2d68a282e8eed51fad13f";
String encAddress = PublicKey.getEncAddress(encPublicKey);
```

### 验签
#### 非静态接口
方法名: verify
注意：调用此方法需要构造PublicKey对象

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| msg | byte[] | 签名原信息
| signMsg | byte[] | 签名后信息

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| verify | boolean | 验签结果

例如：
```java
String publicKey = "b0014085888f15e6fdae80827f5ec129f7e9323cf60732e7f8259fa2d68a282e8eed51fad13f";
PublicKey publicKey = new PublicKey(publicKey);

String src = "test";
String sign = "5EC1B9625D28906378E6ED364855295748A57CDB679F5A23C4EA1427228814B7D68BEDC9D1CCED4720630501C2EA15C9F73639936C95E903432E8E893234C402";
Boolean verifyResult = publicKey.verify(src.getBytes(), sign.getBytes());
```

#### 静态接口
方法名: verify
注意：调用此方法不需要构造PublicKey对象

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| msg | byte[] | 签名原信息
| signMsg | byte[] | 签名后信息
| publicKey | String | 公钥

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| verify | boolean | 验签结果

例如：
```java
String src = "test";
String publicKey = "b0014085888f15e6fdae80827f5ec129f7e9323cf60732e7f8259fa2d68a282e8eed51fad13f";
String sign = "5EC1B9625D28906378E6ED364855295748A57CDB679F5A23C4EA1427228814B7D68BEDC9D1CCED4720630501C2EA15C9F73639936C95E903432E8E893234C402";
Boolean verifyResult = PublicKey.verify(src.getBytes(), sign, publicKey);
```

## 密钥存储器
### 生成密钥存储器
此方法是静态方法

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| encPrivateKey | String | 待存储的密钥，可为null
| password | String | 口令
| n | Integer | CPU消耗参数，可为null
| r | Integer | 内存消息参数，可为null
| p | Integer | 并行化参数，可为null


返回结果：

|变量|类型|描述
|:--- | --- | --- 
| encPrivateKey | String | 已存储的密钥，当参数中encPrivateKey不为null,此值就是入参；否则 ，这是新创建的密钥
| keyStore | JSONObject | 存储密钥的存储器

例如：
```java
String encPrivateKey = "privbtGQELqNswoyqgnQ9tcfpkuH8P1Q6quvoybqZ9oTVwWhS6Z2hi1B";
String password = "test1234";
JSONObject keyStore = new JSONObject();
String returEencPrivateKey = KeyStore.generateKeyStore(encPrivateKey, password, 16384, 8, 1, keyStore);
System.out.println(returEencPrivateKey);
System.out.println(keyStore.toJSONString());
```

### 解析密钥存储器
此方法是静态方法

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| password | String | 口令
| keyStore | JSONObject | 存储密钥的存储器

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| encPrivateKey | String | 解析出来的密钥

例如：
```java
String password = "test1234";
JSONObject keyStore = JSONObject.parseObject("{\"cypher_text\":\"7E0892CAB60761CD8F73A21F0B040ACACAB694AF8C8CA25D4BE8549CCBD8E013AA4C2D338EA11F42596E0EEC05A158C20AE4B51E2B94D102\",\"aesctr_iv\":\"38C33D8E6E5911A0C3F715F5AC75A88A\",\"address\":\"buQdBdkvmAhnRrhLp4dmeCc2ft7RNE51c9EK\",\"scrypt_params\":{\"p\":1,\"r\":8,\"salt\":\"3070E64061711D39A382E23142B91DD9A6B3AB0B5AC1FD4D202191EAC2816661\",\"n\":16384},\"version\":2}");
encPrivateKey = KeyStore.from(keyStore, password);
System.out.println(encPrivateKey);
```

## 助记词
### 生成助记词
此方法是静态方法

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| random | byte[] | 16位字节数组，必须是16位

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| mnemonicCodes | List<String> | 助记词

例如：
```java
byte[] aesIv = new byte[16];
SecureRandom randomIv = new SecureRandom();
randomIv.nextBytes(aesIv);

List<String> mnemonicCodes = Mnemonic.generateMnemonicCode(aesIv);
for (String mnemonicCode : mnemonicCodes) {
	System.out.print(mnemonicCode + " ");
}
System.out.println();
```

### 根据助记词生成私钥
此方法是静态方法

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| mnemonicCodes | List<String> | 助记词
| hdPaths | List<String> | 路径

返回结果：

|变量|类型|描述
|:--- | --- | --- 
| privateKeys | List<String> | 私钥

例如：
```java
List<String> mnemonicCodes = new ArrayList<>();
mnemonicCodes.add("wood");
mnemonicCodes.add("floor");
mnemonicCodes.add("submit");
mnemonicCodes.add("traffic");
mnemonicCodes.add("obvious");
mnemonicCodes.add("indoor");
mnemonicCodes.add("rocket");
mnemonicCodes.add("lunch");
mnemonicCodes.add("melt");
mnemonicCodes.add("park");
mnemonicCodes.add("regular");
mnemonicCodes.add("vessel");

List<String> hdPaths = new ArrayList<>();
hdPaths.add("M/44/80/0/0/1");
List<String> privateKeys = generatePrivateKeys(mnemonicCodes, hdPaths);
for (String privateKey : privateKeys) {
	System.out.print(privateKey + " ");
}
System.out.println();
```

## 计算哈希
方法名：GenerateHashHex
路径：org.bumo.encryption.utils.HashUtil

请求参数：

|变量|类型|描述
|:--- | --- | --- 
| src | byte[] | 待计算的字节数组，即交易的序列化字节数组

例如
```java
String test = "I want to hash test";
String hash = HashUtil.GenerateHashHex(test.getBytes());
```

若要获取bumo底层的hash类型，需要访问http的hello接口，会返回hash类型

### 举例说明
#### 创建账户
测试例子如下：

```java
public static PrivateKey TestCreateAccount(String url, String srcAddress, String srcPrivate, String srcPublic, String signerAddress, String signerPrivate, String signerPublic, KeyType algorithm) {
    PrivateKey bumokey_new = null;
    try {
    	// getAccount
    	String getAccount = url + "/getAccount?address=" + srcAddress;
    	String txSeq = HttpKit.post(getAccount, "");
    	JSONObject tx = JSONObject.parseObject(txSeq);
    	String seq_str = tx.getJSONObject("result").containsKey("nonce") ? tx.getJSONObject("result").getString("nonce") : "0";
    	long nonce = Long.parseLong(seq_str);
    			
    	// generate new Account address, PrivateKey, publicKey
    	bumokey_new = new PrivateKey(algorithm);
    	String newAccountAddress = bumokey_new.getEncAddress();
    			
    	// use src account sign
    	PrivateKey bumoKey_sign = new PrivateKey(signerPrivate);
    			
    	JSONObject transaction = new JSONObject();
    	transaction.put("source_address", srcAddress);
    	transaction.put("nonce", nonce + 1);
    	transaction.put("fee_limit", 1000000);
    	transaction.put("gas_price", 1000);
    	JSONArray operations = new JSONArray();
    	JSONObject operation = new JSONObject();
    	operation.put("type", 1);
    	JSONObject createAccount = new JSONObject();
    	JSONObject priv = new JSONObject();
    	priv.put("master_weight", 1);
    	JSONObject thresholds = new JSONObject();
    	thresholds.put("tx_threshold", 1);
    			
    	createAccount.put("dest_address", newAccountAddress);
    	createAccount.put("init_balance", 1000000000000L);
    	priv.put("thresholds", thresholds);
    	createAccount.put("priv", priv);
    	operation.put("create_account", createAccount);
    	operations.add(operation);
    	transaction.put("operations", operations);
    	String getTransactionBlob = url + "/getTransactionBlob";
    	String blob = HttpKit.post(getTransactionBlob, transaction.toJSONString());
    	JSONObject transactionBlob = JSON.parseObject(blob);
    	long error_code = transactionBlob.getLongValue("error_code");
    	JSONObject blobResult = transactionBlob.getJSONObject("result");
    	if (transactionBlob != null && error_code != 0) {
    		String hash = blobResult.getString("hash");
    		String desc = transactionBlob.getString("error_desc");
    		System.out.println("create account blob (" + hash + ") error description: " + desc);
    		return null;
    	}
    	String blob_hex = blobResult.getString("transaction_blob");
    			
    	// add transaction with signature
    	JSONObject request = new JSONObject();
    	JSONArray items = new JSONArray();
    	JSONObject item = new JSONObject();
    	item.put("transaction_blob", blob_hex);
    	JSONArray signatures = new JSONArray();
    	JSONObject signature = new JSONObject();
    	signature.put("sign_data", HexFormat.byteToHex(bumoKey_sign.sign(HexFormat.hexToByte(blob_hex))));
    	signature.put("public_key", signerPublic);
    	signatures.add(signature);
    	item.put("signatures", signatures);
    	items.add(item);
    	request.put("items", items);
    			
    	String submitTransaction = url + "/submitTransaction";
    	String trans = HttpKit.post(submitTransaction, request.toJSONString());
    	JSONObject transObj = JSONObject.parseObject(trans);
    	JSONArray transResult = transObj.getJSONArray("results");
    	String hash = transResult.getJSONObject(0).getString("hash");
    	if (transResult.getJSONObject(0).getLongValue("error_code") != 0) {
    		String desc = transResult.getJSONObject(0).getString("error_desc");
    		System.out.println("create account transaction(" + hash + ") error description: " + desc);
    		return null;
    	}
    	System.out.println("create account transaction hash (" + hash + ")");
    } catch (Exception e) {
    	e.printStackTrace();
    }
		
    return bumokey_new;
}
```

调用测试例子：

```java
String url = "http://127.0.0.1:36002";
String privateKey = "privbtGQELqNswoyqgnQ9tcfpkuH8P1Q6quvoybqZ9oTVwWhS6Z2hi1B";
String publicKey = "b001b6d3120599d19cae7adb6c5e2674ede8629c871cb8b93bd05bb34d203cd974c3f0bc07e5";
String address = "buQdBdkvmAhnRrhLp4dmeCc2ft7RNE51c9EK";
PrivateKey bumoKey = TestCreateAccount(url, address, privateKey, publicKey, address, privateKey, publicKey, KeyType.ED25519);
System.out.println(bumoKey);
```

#### 发行资产
测试例子如下：
```java
public static void TestIssueAsset(String url, String address, String privateKey, String publicKey, String code, long amount) {
	try {
		// getAccount
		String getAccount = url + "/getAccount?address=" + address;
		String txSeq = HttpKit.post(getAccount, "");
		JSONObject tx = JSONObject.parseObject(txSeq);
		String seq_str = tx.getJSONObject("result").containsKey("nonce") ? tx.getJSONObject("result").getString("nonce") : "0";
		long nonce = Long.parseLong(seq_str);
			
		// use src account sign
		PrivateKey bumoKey_sign = new PrivateKey(privateKey);
			
		JSONObject transaction = new JSONObject();
		transaction.put("source_address", address);
		transaction.put("nonce", nonce + 1);
		transaction.put("fee_limit", 6000000000L);
		transaction.put("gas_price", 1000);
		JSONArray operations = new JSONArray();
		JSONObject operation = new JSONObject();
		operation.put("type", 2);
		JSONObject issueAsset = new JSONObject();
		issueAsset.put("code", code);
		issueAsset.put("amount", amount);
			
		operation.put("issue_asset", issueAsset);
		operations.add(operation);
		transaction.put("operations", operations);
		String getTransactionBlob = url + "/getTransactionBlob";
		String blob = HttpKit.post(getTransactionBlob, transaction.toJSONString());
		JSONObject transactionBlob = JSON.parseObject(blob);
		long error_code = transactionBlob.getLongValue("error_code");
		JSONObject blobResult = transactionBlob.getJSONObject("result");
		if (transactionBlob != null && error_code != 0) {
			String hash = blobResult.getString("hash");
			String desc = transactionBlob.getString("error_desc");
			System.out.println("issue asset blob (" + hash + ") error description: " + desc);
			return;
		}
		String blob_hex = blobResult.getString("transaction_blob");
			
		// add transaction with signature
		JSONObject request = new JSONObject();
		JSONArray items = new JSONArray();
		JSONObject item = new JSONObject();
		item.put("transaction_blob", blob_hex);
		JSONArray signatures = new JSONArray();
		JSONObject signature = new JSONObject();
		signature.put("sign_data", HexFormat.byteToHex(bumoKey_sign.sign(HexFormat.hexToByte(blob_hex))));
		signature.put("public_key", publicKey);
		signatures.add(signature);
		item.put("signatures", signatures);
		items.add(item);
		request.put("items", items);
			
		String submitTransaction = url + "/submitTransaction";
		String trans = HttpKit.post(submitTransaction, request.toJSONString());
		JSONObject transObj = JSONObject.parseObject(trans);
		JSONArray transResult = transObj.getJSONArray("results");
		String hash = transResult.getJSONObject(0).getString("hash");
		if (transResult.getJSONObject(0).getLongValue("error_code") != 0) {
			String desc = transResult.getJSONObject(0).getString("error_desc");
			System.out.println("issue asset transaction(" + hash + ") error description: " + desc);
		}
		System.out.println("issue asset transaction hash (" + hash + ")");
	} catch (Exception e) {
		e.printStackTrace();
	}
}
```

调用测试例子：

```java
String url = "http://127.0.0.1:36002";
String privateKey = "privbtGQELqNswoyqgnQ9tcfpkuH8P1Q6quvoybqZ9oTVwWhS6Z2hi1B";
String publicKey = "b001b6d3120599d19cae7adb6c5e2674ede8629c871cb8b93bd05bb34d203cd974c3f0bc07e5";
String address = "buQdBdkvmAhnRrhLp4dmeCc2ft7RNE51c9EK";
TestIssueAsset(url, address, privateKey, publicKey, "BUH", 10000);
```

#### 转移资产
测试例子如下：

```java
public static void TestPayAsset(String url, String issueAddress, String srcAddress, String srcPrivate, String srcPublic, String destAddress, String code, long amount) {
	try {
			// getAccount
			String getAccount = url + "/getAccount?address=" + srcAddress;
			String txSeq = HttpKit.post(getAccount, "");
			JSONObject tx = JSONObject.parseObject(txSeq);
			String seq_str = tx.getJSONObject("result").containsKey("nonce") ? tx.getJSONObject("result").getString("nonce") : "0";
			long nonce = Long.parseLong(seq_str);
			
			// use src account sign
			PrivateKey bumoKey_sign = new PrivateKey(srcPrivate);
			
			JSONObject transaction = new JSONObject();
			transaction.put("source_address", srcAddress);
			transaction.put("nonce", nonce + 1);
			transaction.put("fee_limit", 1000000);
			transaction.put("gas_price", 1000);
			JSONArray operations = new JSONArray();
			JSONObject operation = new JSONObject();
			operation.put("type", 3);
			JSONObject payAsset = new JSONObject();
			payAsset.put("dest_address", destAddress);
			JSONObject asset = new JSONObject();
			JSONObject key = new JSONObject();
			key.put("issuer", issueAddress);
			key.put("code", code);
			key.put("type", 0);
			
			asset.put("key", key);
			asset.put("amount", amount);
			payAsset.put("asset", asset);
			operation.put("pay_asset", payAsset);
			operations.add(operation);
			transaction.put("operations", operations);
			String getTransactionBlob = url + "/getTransactionBlob";
			String blob = HttpKit.post(getTransactionBlob, transaction.toJSONString());
			JSONObject transactionBlob = JSON.parseObject(blob);
			long error_code = transactionBlob.getLongValue("error_code");
			JSONObject blobResult = transactionBlob.getJSONObject("result");
			if (transactionBlob != null && error_code != 0) {
				String hash = blobResult.getString("hash");
				String desc = transactionBlob.getString("error_desc");
				System.out.println("payAsset blob (" + hash + ") error description: " + desc);
				return;
			}
			String blob_hex = blobResult.getString("transaction_blob");
			
			// add transaction with signature
			JSONObject request = new JSONObject();
			JSONArray items = new JSONArray();
			JSONObject item = new JSONObject();
			item.put("transaction_blob", blob_hex);
			JSONArray signatures = new JSONArray();
			JSONObject signature = new JSONObject();
			signature.put("sign_data", HexFormat.byteToHex(bumoKey_sign.sign(HexFormat.hexToByte(blob_hex))));
			signature.put("public_key", srcPublic);
			signatures.add(signature);
			item.put("signatures", signatures);
			items.add(item);
			request.put("items", items);
			
			String submitTransaction = url + "/submitTransaction";
			String trans = HttpKit.post(submitTransaction, request.toJSONString());
			JSONObject transObj = JSONObject.parseObject(trans);
			JSONArray transResult = transObj.getJSONArray("results");
			String hash = transResult.getJSONObject(0).getString("hash");
			if (transResult.getJSONObject(0).getLongValue("error_code") != 0) {
				String desc = transResult.getJSONObject(0).getString("error_desc");
				System.out.println("payAsset transaction(" + hash + ") error description: " + desc);
			}
			System.out.println("payAsset transaction hash (" + hash + ")");
		} catch (Exception e) {
			e.printStackTrace();
		}
}
```

调用测试例子：

```java
String url = "http://127.0.0.1:36002";
String privateKey = "privbtGQELqNswoyqgnQ9tcfpkuH8P1Q6quvoybqZ9oTVwWhS6Z2hi1B";
String publicKey = "b001b6d3120599d19cae7adb6c5e2674ede8629c871cb8b93bd05bb34d203cd974c3f0bc07e5";
String address = "buQdBdkvmAhnRrhLp4dmeCc2ft7RNE51c9EK";
String toAddress = "buQgQPcxaPg8jNiBj19DFhKH1ewjkmZsBqj4";
TestPayAsset(url, address, address, privateKey, publicKey, toAddress, "BUH", 5000);
```

#### 转移bu资产
测试例子如下 ：

```java
public static void TestPayCoin(String url, String srcAddress, String srcPrivate, String srcPublic, String destAddress, long amount) {
	try {
		// getAccount
		String getAccount = url + "/getAccount?address=" + srcAddress;
		String txSeq = HttpKit.post(getAccount, "");
		JSONObject tx = JSONObject.parseObject(txSeq);
		String seq_str = tx.getJSONObject("result").containsKey("nonce") ? tx.getJSONObject("result").getString("nonce") : "0";
		long nonce = Long.parseLong(seq_str);
			
		// use src account sign
		PrivateKey bumoKey_sign = new PrivateKey(srcPrivate);
			
		JSONObject transaction = new JSONObject();
		transaction.put("source_address", srcAddress);
		transaction.put("nonce", nonce + 1);
		transaction.put("fee_limit", 1000000);
		transaction.put("gas_price", 1000);
		JSONArray operations = new JSONArray();
		JSONObject operation = new JSONObject();
		operation.put("type", 7);
		JSONObject payCoin = new JSONObject();
		payCoin.put("dest_address", destAddress);
		payCoin.put("amount", amount);
			
		operation.put("pay_coin", payCoin);
		operations.add(operation);
		transaction.put("operations", operations);
		String getTransactionBlob = url + "/getTransactionBlob";
		String blob = HttpKit.post(getTransactionBlob, transaction.toJSONString());
		JSONObject transactionBlob = JSON.parseObject(blob);
		long error_code = transactionBlob.getLongValue("error_code");
		JSONObject blobResult = transactionBlob.getJSONObject("result");
		if (transactionBlob != null && error_code != 0) {
			String hash = blobResult.getString("hash");
			String desc = transactionBlob.getString("error_desc");
			System.out.println("pay coin blob (" + hash + ") error description: " + desc);
			return;
		}
		String blob_hex = blobResult.getString("transaction_blob");
			
		// add transaction with signature
		JSONObject request = new JSONObject();
		JSONArray items = new JSONArray();
		JSONObject item = new JSONObject();
		item.put("transaction_blob", blob_hex);
		JSONArray signatures = new JSONArray();
		JSONObject signature = new JSONObject();
		signature.put("sign_data", HexFormat.byteToHex(bumoKey_sign.sign(HexFormat.hexToByte(blob_hex))));
		signature.put("public_key", srcPublic);
		signatures.add(signature);
		item.put("signatures", signatures);
		items.add(item);
		request.put("items", items);
			
		String submitTransaction = url + "/submitTransaction";
		String trans = HttpKit.post(submitTransaction, request.toJSONString());
		JSONObject transObj = JSONObject.parseObject(trans);
		JSONArray transResult = transObj.getJSONArray("results");
		String hash = transResult.getJSONObject(0).getString("hash");
		if (transResult.getJSONObject(0).getLongValue("error_code") != 0) {
			String desc = transResult.getJSONObject(0).getString("error_desc");
			System.out.println("pay coin transaction(" + hash + ") error description: " + desc);
		}
		System.out.println("pay coin transaction hash (" + hash + ")");
	} catch (Exception e) {
		e.printStackTrace();
	}
}
```

调用测试例子：

```java
String url = "http://127.0.0.1:36002";
String privateKey = "privbtGQELqNswoyqgnQ9tcfpkuH8P1Q6quvoybqZ9oTVwWhS6Z2hi1B";
String publicKey = "b001b6d3120599d19cae7adb6c5e2674ede8629c871cb8b93bd05bb34d203cd974c3f0bc07e5";
String address = "buQdBdkvmAhnRrhLp4dmeCc2ft7RNE51c9EK";
String toAddress = "buQgQPcxaPg8jNiBj19DFhKH1ewjkmZsBqj4";
TestPayCoin(url, address, privateKey, publicKey, toAddress, 50000);
```
