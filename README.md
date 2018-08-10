# Getting Started with the TTT Wallet Android SDK
---

* [Before you start](#start)
* [Install](#GradleIntegration)
* [Android code obfuscation](#obfuscation)
* [Api Details](#apiDetails)

### <a name="start">Before You Start</a>
TTT wallet is Fast, Light, Sweet crypto wallet, We recommend below document to understand the basic idea of TTT wallet
- [TrustNote White Paper](https://github.com/trustnote/document)
- [Bip39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)
- [Bip44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki)
- [Bip32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)


### <a name="GradleIntegration">Install</a>  

Integrating the SDK into your project manually

* [Download the latest SDK](https://github.com/TrustNoteDevelopers/android_sdk/raw/master/ttt_wallet_sdk_v0.1.aar)
* Extract the ttt_wallet_sdk_v***.aar package from the zip file
* Copy the aar to module's libs folder
* Add below code in the module's build.gradle：

``` 
    dependencies {
        implementation files('libs/ttt_wallet_sdk_v0.1.aar')
    }
```
### <a name="obfuscation">Android code obfuscation</a> 

If you are using ProGuard in your project, you must add the following code to your ProGuard file:

``` java
    -keep public class org.trustnote.wallet.sdk.**{*;}
    -dontwarn org.trustnote.wallet.sdk.**

    #for js and webview interface
    -keepclassmembers class * {
        @android.webkit.JavascriptInterface <methods>;
    }
```
    

### <a name="apiDetails">API Details</a>  

> TTTWalletSDK summary

```
    /**
     * SDK entry point, include all api method.
     * 1. Api method should be called from non UI thread except the init method.
     * 2. Err code: if method return "0", that's mean something was wrong.
     */
    public class TTTWalletSDK
 
```


> init

```
    /**
     * @method  initialize the SDK
     */
    public static void init(Context context)
 
```

> createMnemonic

```
    /**
     * @method  generate random bip39 mnemonic
     * @param   void
     * @return  string with 12 words, separated by space.
     * @Ref: https://github.com/bitcoin/bips/blob/master/bip-0039/bip-0039-wordlists.md
     */
    public static String createMnemonic()
```

> createPrivateKey

```
    /**
     * @method  generate private key from mnemonic
     * @param   mnemonic
     * @return  private key
     */
    public static String createPrivateKey(String mnemonic)
```

> createWallet

```
    /**
     * @method  create wallet with private key
     * @param   seed or pri-key
     * @return  wallet pub-key
     */
    public static String createWallet(String privkey)
```

> createPublickey

```
    /**
     * @method  create a pub-key from wallet pub-key
     * @param   walletPubkey
     * @return  pub-key
     */
    public static String createPublickey(String walletPubkey)
```

> createAddress

```
    /**
     * @method  create an address from wallet pub-key
     * @param   walletPubkey
     * @return  address
     */
    public static String createAddress(String walletPubkey)
```

> isValidAddress

```
    /**
     * @method  check address is valid TTT address
     * @param   address
     * @return  boolean
     */
    public static boolean isValidAddress(String address)
```

> sign

```
    /**
     * @method  sign unit hash with pri-key
     * @param
     * @return  signature
     */
    public static String sign(String priKey, String unitHash)
```

> verify

```
    /**
     * @method  verify the signature
     * @param
     * @return  boolean -- true: signature is ok, otherwise false.
     */
    public static String verify(String publicKey, String signature, String unitHash)
```

