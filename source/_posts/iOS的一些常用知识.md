---
title: iOS的一些常用知识
date: 2017-08-18 15:01:14
tags:
---
> 最近在写一个iOS的项目,现在记录下一些用到的知识点,哎 我的知识盲区有点多啊

### 1.类扩展 Categires
####  1.1 颜色相关
```sh
+ (UIColor *)colorFromHexString:(NSString *)hexString {
    unsigned rgbValue = 0;
    NSScanner *scanner = [NSScanner scannerWithString:hexString];
    [scanner setScanLocation:1]; // bypass '#' character
    [scanner scanHexInt:&rgbValue];
    return [UIColor colorWithRed:((rgbValue & 0xFF0000) >> 16)/255.0 green:((rgbValue & 0xFF00) >> 8)/255.0 blue:(rgbValue & 0xFF)/255.0 alpha:1.0];
}
```
#### 1.2 安全类
### 类 <h4>类<h4> <h5>类<h5> ...
```sh
//
//  NSData+LYSecurity.h
//  testAES
//
//  Created by 李越(EX-LIYUE003) on 2017/8/22.
//  Copyright © 2017年 李越(EX-LIYUE003). All rights reserved.
//

#import <Foundation/Foundation.h>
/**
 Hash 校验算法
 */
typedef enum : NSUInteger {
    //md2 16字节长度
    CCDIGEST_MD2 = 1000,
    //md4 16字节长度
    CCDIGEST_MD4,
    //md5 16字节长度
    CCDIGEST_MD5,
    //sha1 20字节长度
    CCDIGEST_SHA1,
    //SHA224 28字节长度
    CCDIGEST_SHA224,
    //SHA256 32字节长度
    CCDIGEST_SHA256,
    //SHA384 48字节长度
    CCDIGEST_SHA384,
    //SHA512 64字节长度
    CCDIGEST_SHA512,
} CCDIGESTAlgorithm;



/**
 
 RSA算法填充blockSize
 */
typedef enum : NSUInteger {
    //不填充，最大数据块为 blockSize
    RSAPaddingNONE,
    //填充方式pkcs1,最大数据块为 blockSize -11
    RSAPaddingPKCS1,
    //填充方式OAEP, 最大数据块为 blockSize -42
    RSAPaddingOAEP,
} RSAPaddingTYPE;


//主要使用PKCS1 方式的填充，最大签名数据长度为blockSize-11
//签名数据 一般签名，数据的HASH值；
//签名算法从ios5以后不再支持md5,md2
typedef enum : NSUInteger {
    SEC_PKCS1SHA1 = 2000,
    SEC_PKCS1SHA224,
    SEC_PKCS1SHA256,
    SEC_PKCS1SHA384,
    SEC_PKCS1SHA512,
} SEC_PKCS1_ALGORITHM;

@interface NSData (LYSecurity)
#pragma - mark - AES 加密
/**
 支持的AES key size 有 128位，192位，256位
 数据填充方式：kCCOptionPKCS7Padding
 分组模式：cbc,ecb
 */
- (NSData *)AES_CBC_EncryptWith:(NSData *)key iv:(NSData *)iv;

/**
 AES cbc 模式解密，
 @key 长度16字节，24字节，32字节
 @iv 16字节
 */
- (NSData *)AES_CBC_DecryptWith:(NSData *)key iv:(NSData *)iv;

/**
 AES ecb 模式加密，
 @key 长度16字节，24字节，32字节
 */
- (NSData *)AES_ECB_EncryptWith:(NSData *)key;

/**
 AES ecb 模式解密，
 @key 长度16字节，24字节，32字节
 */
- (NSData *)AES_ECB_DecryptWith:(NSData *)key;


#pragma - mark - HASH校验

/**
 根据不同算法计算Hash值

 @param ccAlgorithm hash算法
 @return 结果
 */
- (NSData *)hashDataWith:(CCDIGESTAlgorithm )ccAlgorithm;


/**
 返回HashString

 @return 结果
 */
- (NSString *)hexString;

#pragma - mark - RSA加密
/**
 公钥加密
 */
- (NSData *)RSAEncryptWith:(SecKeyRef )publicKey paddingType:(RSAPaddingTYPE )pdType;

/**
 私钥解密
 */
- (NSData *)RSADecryptWith:(SecKeyRef )privateKey paddingType:(RSAPaddingTYPE )pdType;

#pragma - mark - 
/**
 根据不同的算法，签名数据，
 */
- (NSData *)signDataWith:(SecKeyRef)privateKey algorithm:(SEC_PKCS1_ALGORITHM )ccAlgorithm;

/**
 验证签名数据
 */
- (BOOL)verifySignWith:(SecKeyRef)publicKey signData:(NSData *)signData algorithm:(SEC_PKCS1_ALGORITHM )ccAlgorithm;

@end

```

```sh
//
//  NSData+LYSecurity.m
//  testAES
//
//  Created by 李越(EX-LIYUE003) on 2017/8/22.
//  Copyright © 2017年 李越(EX-LIYUE003). All rights reserved.
//

#import "NSData+LYSecurity.h"
#import <CommonCrypto/CommonCryptor.h>
#import <CommonCrypto/CommonDigest.h>
#import <Security/SecBase.h>

@implementation NSData (LYSecurity)

#pragma - mark - AES加密
- (NSData *)AES_CBC_EncryptWith:(NSData *)key iv:(NSData *)iv
{
    NSData *retData = nil;
    NSUInteger dataLength = [self length];
    size_t bufferSize = dataLength + kCCBlockSizeAES128;
    void *buffer = malloc(bufferSize);
    bzero(buffer, bufferSize);
    size_t numBytesEncrypted = 0;
    CCCryptorStatus cryptStatus = CCCrypt(kCCEncrypt,
                                          kCCAlgorithmAES128,
                                          kCCOptionPKCS7Padding,
                                          key.bytes,
                                          key.length,
                                          iv.bytes,
                                          self.bytes, self.length,
                                          buffer, bufferSize,
                                          &numBytesEncrypted);
    if (cryptStatus == kCCSuccess) {
        retData = [NSData dataWithBytes:buffer length:numBytesEncrypted];
    }
    free(buffer);
    return retData;
    
}

- (NSData *)AES_CBC_DecryptWith:(NSData *)key iv:(NSData *)iv
{
    NSData *retData = nil;
    NSUInteger dataLength = [self length];
    size_t bufferSize = dataLength + kCCBlockSizeAES128;
    void *buffer = malloc(bufferSize);
    bzero(buffer, bufferSize);
    size_t numBytesEncrypted = 0;
    CCCryptorStatus cryptStatus = CCCrypt(kCCDecrypt, kCCAlgorithmAES128,
                                          kCCOptionPKCS7Padding,
                                          key.bytes, key.length,
                                          iv.bytes,
                                          self.bytes, self.length,
                                          buffer, bufferSize,
                                          &numBytesEncrypted);
    if (cryptStatus == kCCSuccess) {
        retData = [NSData dataWithBytes:buffer length:numBytesEncrypted];
    }
    free(buffer);
    return retData;
}


- (NSData *)AES_ECB_EncryptWith:(NSData *)key
{
    NSData *retData = nil;
    NSUInteger dataLength = [self length];
    size_t bufferSize = dataLength + kCCBlockSizeAES128;
    void *buffer = malloc(bufferSize);
    bzero(buffer, bufferSize);
    size_t numBytesEncrypted = 0;
    CCCryptorStatus cryptStatus = CCCrypt(kCCEncrypt, kCCAlgorithmAES128,
                                          kCCOptionPKCS7Padding|kCCOptionECBMode,
                                          key.bytes, key.length,
                                          NULL,
                                          self.bytes, self.length,
                                          buffer, bufferSize,
                                          &numBytesEncrypted);
    if (cryptStatus == kCCSuccess) {
        retData = [NSData dataWithBytes:buffer length:numBytesEncrypted];
    }
    free(buffer);
    return retData;
}

- (NSData *)AES_ECB_DecryptWith:(NSData *)key
{
    NSData *retData = nil;
    NSUInteger dataLength = [self length];
    size_t bufferSize = dataLength + kCCBlockSizeAES128;
    void *buffer = malloc(bufferSize);
    bzero(buffer, bufferSize);
    size_t numBytesEncrypted = 0;
    CCCryptorStatus cryptStatus = CCCrypt(kCCDecrypt, kCCAlgorithmAES128,
                                          kCCOptionPKCS7Padding|kCCOptionECBMode,
                                          key.bytes, key.length,
                                          NULL,
                                          self.bytes, self.length,
                                          buffer, bufferSize,
                                          &numBytesEncrypted);
    if (cryptStatus == kCCSuccess) {
        retData = [NSData dataWithBytes:buffer length:numBytesEncrypted];
    }
    free(buffer);
    return retData;
}

#pragma -mark - 计算数据的Hash值
- (NSData *)hashDataWith:(CCDIGESTAlgorithm )ccAlgorithm
{
    NSData *retData = nil;
    if (self.length <1) {
        return nil;
    }
    
    unsigned char *md;
    
    switch (ccAlgorithm) {
        case CCDIGEST_MD2:
        {
            md = malloc(CC_MD2_DIGEST_LENGTH);
            bzero(md, CC_MD2_DIGEST_LENGTH);
            CC_MD2(self.bytes, (CC_LONG)self.length, md);
            retData = [NSData dataWithBytes:md length:CC_MD2_DIGEST_LENGTH];
        }
            break;
        case CCDIGEST_MD4:
        {
            md = malloc(CC_MD4_DIGEST_LENGTH);
            bzero(md, CC_MD4_DIGEST_LENGTH);
            CC_MD4(self.bytes, (CC_LONG)self.length, md);
            retData = [NSData dataWithBytes:md length:CC_MD4_DIGEST_LENGTH];
            
        }
            break;
        case CCDIGEST_MD5:
        {
            md = malloc(CC_MD5_DIGEST_LENGTH);
            bzero(md, CC_MD5_DIGEST_LENGTH);
            CC_MD5(self.bytes, (CC_LONG)self.length, md);
            retData = [NSData dataWithBytes:md length:CC_MD5_DIGEST_LENGTH];
            
        }
            break;
        case CCDIGEST_SHA1:
        {
            md = malloc(CC_SHA1_DIGEST_LENGTH);
            bzero(md, CC_SHA1_DIGEST_LENGTH);
            CC_SHA1(self.bytes, (CC_LONG)self.length, md);
            retData = [NSData dataWithBytes:md length:CC_SHA1_DIGEST_LENGTH];
            
        }
            break;
        case CCDIGEST_SHA224:
        {
            md = malloc(CC_SHA224_DIGEST_LENGTH);
            bzero(md, CC_SHA224_DIGEST_LENGTH);
            CC_SHA224(self.bytes, (CC_LONG)self.length, md);
            retData = [NSData dataWithBytes:md length:CC_SHA224_DIGEST_LENGTH];
            
        }
            break;
        case CCDIGEST_SHA256:
        {
            md = malloc(CC_SHA256_DIGEST_LENGTH);
            bzero(md, CC_SHA256_DIGEST_LENGTH);
            CC_SHA256(self.bytes, (CC_LONG)self.length, md);
            retData = [NSData dataWithBytes:md length:CC_SHA256_DIGEST_LENGTH];
            
        }
            break;
        case CCDIGEST_SHA384:
        {
            md = malloc(CC_SHA384_DIGEST_LENGTH);
            bzero(md, CC_SHA384_DIGEST_LENGTH);
            CC_SHA384(self.bytes, (CC_LONG)self.length, md);
            retData = [NSData dataWithBytes:md length:CC_SHA384_DIGEST_LENGTH];
            
        }
            break;
        case CCDIGEST_SHA512:
        {
            md = malloc(CC_SHA512_DIGEST_LENGTH);
            bzero(md, CC_SHA512_DIGEST_LENGTH);
            CC_SHA512(self.bytes, (CC_LONG)self.length, md);
            retData = [NSData dataWithBytes:md length:CC_SHA512_DIGEST_LENGTH];
            
        }
            break;
            
        default:
            md = malloc(1);
            break;
    }
    
    free(md);
    md = NULL;
    
    return retData;
    
}


- (NSString *)hexString
{
    NSMutableString *result = nil;
    if (self.length <1) {
        return nil;
    }
    result = [[NSMutableString alloc] initWithCapacity:self.length * 2];
    for (size_t i = 0; i < self.length; i++) {
        [result appendFormat:@"%02x", ((const uint8_t *) self.bytes)[i]];
    }
    return result;
}


+ (NSData *)dataWithHexString:(NSString *)hexString {
    NSMutableData *     result;
    NSUInteger          cursor;
    NSUInteger          limit;
    
    NSParameterAssert(hexString != nil);
    
    result = nil;
    cursor = 0;
    limit = hexString.length;
    if ((limit % 2) == 0) {
        result = [[NSMutableData alloc] init];
        
        while (cursor != limit) {
            unsigned int    thisUInt;
            uint8_t         thisByte;
            
            if ( sscanf([hexString substringWithRange:NSMakeRange(cursor, 2)].UTF8String, "%x", &thisUInt) != 1 ) {
                result = nil;
                break;
            }
            thisByte = (uint8_t) thisUInt;
            [result appendBytes:&thisByte length:sizeof(thisByte)];
            cursor += 2;
        }
    }
    
    return result;
}

#pragma - mark - RSA 加密
/**
 公钥加密
 */
- (NSData *)RSAEncryptWith:(SecKeyRef )publicKey paddingType:(RSAPaddingTYPE )pdType
{
    if (!publicKey || self.length <1) {
        return nil;
    }
    OSStatus ret;
    NSData *retData = nil;
    size_t blockSize = SecKeyGetBlockSize(publicKey);
    uint8_t *encData = malloc(blockSize);
    bzero(encData, blockSize);
    
    SecPadding rsaPdd;
    switch (pdType) {
        case RSAPaddingNONE:
            rsaPdd = kSecPaddingNone;
            break;
        case RSAPaddingPKCS1:
            rsaPdd = kSecPaddingPKCS1;
            break;
        case RSAPaddingOAEP:
            rsaPdd = kSecPaddingOAEP;
            break;
            
        default:
            rsaPdd = kSecPaddingPKCS1;
            break;
    }
    
    ret = SecKeyEncrypt(publicKey, rsaPdd, self.bytes, self.length, encData, &blockSize);
    if (ret==errSecSuccess) {
        retData = [NSData dataWithBytes:encData length:blockSize];
    }
    free(encData);
    encData = NULL;
    
    return retData;
}

/**
 私钥解密
 */
- (NSData *)RSADecryptWith:(SecKeyRef )privateKey paddingType:(RSAPaddingTYPE )pdType
{
    if (!privateKey || self.length <1) {
        return nil;
    }
    NSData *retData = nil;
    OSStatus ret;
    size_t blockSize = SecKeyGetBlockSize(privateKey);
    uint8_t *decData = malloc(blockSize);
    bzero(decData, blockSize);
    SecPadding rsaPdd;
    switch (pdType) {
        case RSAPaddingNONE:
            rsaPdd = kSecPaddingNone;
            break;
        case RSAPaddingPKCS1:
            rsaPdd = kSecPaddingPKCS1;
            break;
        case RSAPaddingOAEP:
            rsaPdd = kSecPaddingOAEP;
            break;
            
        default:
            rsaPdd = kSecPaddingPKCS1;
            break;
    }
    
    ret = SecKeyDecrypt(privateKey, rsaPdd, self.bytes, self.length, decData, &blockSize);
    if (ret==errSecSuccess) {
        retData = [NSData dataWithBytes:decData length:blockSize];
    }
    free(decData);
    decData = NULL;
    return retData;
}

#pragma - mark - 签名验证
/**
 根据不同的算法，签名数据，
 */
- (NSData *)signDataWith:(SecKeyRef)privateKey algorithm:(SEC_PKCS1_ALGORITHM )ccAlgorithm
{
    if (!privateKey || self.length <1) {
        return nil;
    }
    
    OSStatus ret;
    NSData *retData = nil;
    size_t siglen = SecKeyGetBlockSize(privateKey);
    uint8_t *sig = malloc(siglen);
    bzero(sig, siglen);
    
    SecPadding secpdal ;
    switch (ccAlgorithm) {
        case SEC_PKCS1SHA1:
            secpdal = kSecPaddingPKCS1SHA1;
            break;
        case SEC_PKCS1SHA224:
            secpdal = kSecPaddingPKCS1SHA224;
            break;
        case SEC_PKCS1SHA256:
            secpdal = kSecPaddingPKCS1SHA256;
            break;
        case SEC_PKCS1SHA384:
            secpdal = kSecPaddingPKCS1SHA384;
            break;
        case SEC_PKCS1SHA512:
            secpdal = kSecPaddingPKCS1SHA512;
            break;
        default:
            secpdal = kSecPaddingPKCS1SHA1;
            break;
    }
    
    ret = SecKeyRawSign(privateKey, secpdal, self.bytes, self.length, sig, &siglen);
    if (ret==errSecSuccess) {
        retData = [NSData dataWithBytes:sig length:siglen];
    }
    
    free(sig);
    sig = NULL;
    
    return retData;
}

/**
 验证签名
 */
- (BOOL)verifySignWith:(SecKeyRef)publicKey signData:(NSData *)signData algorithm:(SEC_PKCS1_ALGORITHM )ccAlgorithm
{
    if (!publicKey || self.length <1) {
        return NO;
    }
    OSStatus ret;
    BOOL retStatus = NO;
    SecPadding secpdal ;
    switch (ccAlgorithm) {
        case SEC_PKCS1SHA1:
            secpdal = kSecPaddingPKCS1SHA1;
            break;
        case SEC_PKCS1SHA224:
            secpdal = kSecPaddingPKCS1SHA224;
            break;
        case SEC_PKCS1SHA256:
            secpdal = kSecPaddingPKCS1SHA256;
            break;
        case SEC_PKCS1SHA384:
            secpdal = kSecPaddingPKCS1SHA384;
            break;
        case SEC_PKCS1SHA512:
            secpdal = kSecPaddingPKCS1SHA512;
            break;
        default:
            secpdal = kSecPaddingPKCS1SHA1;
            break;
    }
    ret = SecKeyRawVerify(publicKey, secpdal, self.bytes, self.length,signData.bytes, signData.length);
    if (ret==errSecSuccess) {
        retStatus = YES;
    }
    return retStatus;
}


@end

```

```sh
typedef NS_ENUM(NSInteger,LYEncryptKeyOption) {
    ASCIICapable,//94个字符
    LetterAndNum,//大小写字母加数字
    Num,//纯数字
};
#pragma - mark - 生成随机加密Key
+ (NSString *)currentRandomKeyStringWithLength:(NSInteger)Length AndKeyOption:(LYEncryptKeyOption)EncryptKeyOption
{
    
    NSString *string = [[NSString alloc]init];
    for (int i = 0; i < Length; i++) {
        switch (EncryptKeyOption) {
            case ASCIICapable:
            {
                int figure = arc4random() % 94+33;
                NSString *tempString = [NSString stringWithFormat:@"%c", figure];
                string = [string stringByAppendingString:tempString];
            }
                break;
            case LetterAndNum:
            {
                int number = arc4random() % 36;
                if (number < 10) {
                    int figure = arc4random() % 10;
                    NSString *tempString = [NSString stringWithFormat:@"%d", figure];
                    string = [string stringByAppendingString:tempString];
                }else {
                    int figure = (arc4random() % 26) + 97;
                    char character = figure;
                    NSString *tempString =arc4random()%2 ? [[NSString stringWithFormat:@"%c", character] uppercaseString]:[[NSString stringWithFormat:@"%c", character] lowercaseString];
                    string = [string stringByAppendingString:tempString];
                }
            }
                break;
            case Num:
            {
                int figure = arc4random() % 10;
                NSString *tempString = [NSString stringWithFormat:@"%d", figure];
                string = [string stringByAppendingString:tempString];
            }
                break;
 
            default:
                break;
        }
            }
    return string;
    
}
```

### 2. 冷知识
 * 按键的状态
 
 1.动态按钮(按压和手指移开相应不同方法)
 
```sh
 [Button addTarget:self action:@selector(actionByBtn:) forControlEvents:UIControlEventTouchDown];
 [Button addTarget:self action:@selector(actionByBtn:) forControlEvents:UIControlEventTouchUpInside|UIControlEventTouchUpOutside];
```
* textfield过滤空格

```sh
-(BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string{
    //
    NSString *tem = [[string componentsSeparatedByCharactersInSet:[NSCharacterSet whitespaceCharacterSet]]componentsJoinedByString:@""];
    if (![string isEqualToString:tem]) {
//        [SVProgressHUD showErrorWithStatus:@"账号用户名不含有空格"];
//        [SVProgressHUD dismissWithDelay:0.3];
        MBProgressHUD *HUD = [MBProgressHUD showHUDAddedTo:self animated:YES];
        HUD.mode = MBProgressHUDModeText;
        HUD.label.text = @"输入不能包含有空格";
        HUD.removeFromSuperViewOnHide = YES;
        [HUD hideAnimated:YES afterDelay:1.0f];
        
        return NO;
        
    }
    return YES;
}

```



* 计算String的字节数

```sh
- (NSUInteger)lengthOfBytesUsingEncoding:(NSStringEncoding)enc;
```
* String 替换
* 
```sh
- (void)viewDidLoad {
    [super viewDidLoad];
   
    NSString *str1 = @"<hello,wo  r d!>";
    //删除字符串两端的尖括号
    NSMutableString *mString = [NSMutableString stringWithString:str1];
    //第一个参数是要删除的字符的索引，第二个是从此位开始要删除的位数
    [mString deleteCharactersInRange:NSMakeRange(0, 1)];
    [mString deleteCharactersInRange:NSMakeRange(mString.length-1, 1)];
    NSLog(@"mString:%@",mString);
    
    //删除字符串中的空格
    NSString *str2 = [mString stringByReplacingOccurrencesOfString:@" " withString:@""];
    NSLog(@"str2:%@",str2);
    
    //同样的可以替换字符
    NSString *str3 = [str2 stringByReplacingOccurrencesOfString:@"," withString:@"好"];
    NSLog(@"str3:%@",str3);
    
    //替换某一位置的字符
    NSString *str4 = [str3 stringByReplacingCharactersInRange:NSMakeRange(0, 1) withString:@"哈哈"];
    NSLog(@"str4:%@",str4);
}
```
* String  Data Base64转换

 ```sh
 NSData.h
 /* Create an NSData from a Base-64, UTF-8 encoded NSData. By default, returns nil when the input is not recognized as valid Base-64.
*/
- (nullable instancetype)initWithBase64EncodedData:(NSData *)base64Data options:(NSDataBase64DecodingOptions)options NS_AVAILABLE(10_9, 7_0);
 /* Create an NSData from a Base-64 encoded NSString using the given options. By default, returns nil when the input is not recognized as valid Base-64.
*/
- (nullable instancetype)initWithBase64EncodedString:(NSString *)base64String options:(NSDataBase64DecodingOptions)options NS_AVAILABLE(10_9, 7_0);
/* Create a Base-64 encoded NSString from the receiver's contents using the given options.
*/
- (NSString *)base64EncodedStringWithOptions:(NSDataBase64EncodingOptions)options NS_AVAILABLE(10_9, 7_0);
 ```
 