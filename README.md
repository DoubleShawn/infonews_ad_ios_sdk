# infonews_ad_ios_sdk
一. 创建广告位，获取广告位ID
1. 获取APPID 和 广告位ID
2. 根据需求配置广告位

二. 初始化SDK

需要导入：SINSSPADFramework.framework  和 SINAdSDK.bundle
 
在app的冷启动的时候注册广告：
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions{
     [SINAdManager registerWithAppid:KAppID];
}

三. 初始化SDK
1.开屏广告
初始化方法：
/**
 初始化开屏广告的方法
 @param adID : 广告ID，必传
 @param frame : splashAd的frame. 建议按照手机屏幕大小传入（UIScreen.main.bounds）
 @return SINAdSplashView
 */
- (instancetype)initWithAdID:(NSString *)adID frame:(CGRect)frame;
- (void)loadAdData;  （在这之前，必须要传入rootVC，用于处理广告操作的根视图控制器。如果不传落地页无法展示）

以下是回调的方法：
 广告数据请求失败
- (void)nativeAd:(SINAdSplashView *)splash didFailWithError:(NSError *_Nullable)error;

 广告已经展示在屏幕上
- (void)splashDidPresentScreen:(SINAdSplashView *)splash;

 广告即将在屏幕上消失
- (void)splashWillDismissScreen:(SINAdSplashView *)splash;

 开屏广告已经在屏幕上消失
- (void)splashDidDismissScreen:(SINAdSplashView *)splash;

 广告落地页，消失的时候调用 （如果开屏广告未点击，开屏广告已经在屏幕上消失也会调用） (暂时未加入)
- (void)splashLandingPageDidClose:(SINAdSplashView *)splash;

 广告被点击
- (void)splashDidClick:(SINAdSplashView *)splash;

 自定义品牌区域视图
 可根据UI类型，返回不同样式的logo视图
- (UIView *)splash:(SINAdSplashView *)splash logoViewForUIType:(SINAdSplashUIType)type;

2.横幅广告
初始化方法，需要用SINAdRelatedViewManager 来进行加载。允许一次多条广告的拉取。
 @param size 预期广告视图大小，当大小为零时，高度将变成最适合的高度。（SINAdRelatedView可以取到）
- (instancetype)initWithAdConfig:(SINAdConfig *)adConfig adSize:(CGSize)size;
 加载广告（最多传入3）
- (void)loadAd:(NSInteger)count;

回调方法：
 * 加载广告时成功回调 （SINAdRelatedView 就是渲染View，直接展示即可。）
- (void)nativeExpressAdSuccessToLoad:(SINAdRelatedViewManager *)nativeExpressAd views:(NSArray<__kindof SINAdRelatedView *> *)views;

 * 加载广告失败回调
- (void)nativeExpressAdFailToLoad:(SINAdRelatedViewManager *)nativeExpressAd error:(NSError *_Nullable)error;

 当relatedView视图广告成功加载时，将调用此方法。
- (void)relatedViewDidLoad:(SINAdRelatedView *)relatedView WithAdmodel:(SINNativeAd *_Nullable)nativeAd;

 当RelatedView广告加载失败时，将调用此方法。
- (void)relatedViewRenderFail:(SINAdRelatedView *)relatedView Error:(NSError *_Nullable)error;

 当RelatedView广告变得可视，将调用此方法。
- (void)relatedViewDidBecomVisible:(SINAdRelatedView *)relatedView WithAdmodel:(SINNativeAd *_Nullable)nativeAd;

 单击RelatedView时将调用此方法。
- (void)relatedViewDidClick:(SINAdRelatedView *)relatedView WithAdmodel:(SINNativeAd *_Nullable)nativeAd;
2.需要自渲染：
SINNativeAd
初始化方法
- (instancetype)initWithAdConfig:(SINAdConfig *)adConfig;

 Ad Config description. 用于生成广告的配置的参数
@property (nonatomic, strong, readwrite, nullable) SINAdConfig *adConfig;
