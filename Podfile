platform :ios,'9.0'
inhibit_all_warnings!
# use_frameworks!
source 'https://github.com/CocoaPods/Specs.git'

def common_pods_all
  pod 'AFNetworking', '~> 4.0.1'
  pod 'SDWebImage','~> 5.9.4'
  # gif
  pod 'SDWebImageFLPlugin', '~> 0.4.0'
  pod 'SDWebImageWebPCoder', '~> 0.6.1'
  pod 'FLAnimatedImage', '~> 1.0.12'
  pod 'Masonry', '~> 1.1.0'
  pod 'NullSafe', '~> 2.0'
  pod 'MJExtension', '~> 3.2.1'
  pod 'MJRefresh', '~> 3.4.3'
  pod 'FMDB', '~> 2.7.5'
  pod 'IQKeyboardManager', '~> 6.5.6'
  pod 'YYText', '~> 1.0.7'
  pod 'TYCyclePagerView', '~> 1.2.0'
  pod 'UICKeyChainStore', '~> 2.2.0'
  pod 'CTMediator', '~> 32'
  pod 'FDFullscreenPopGesture', '~> 1.1'
  pod 'GKPhotoBrowser', '~> 2.1.1'
  # pod 'TZImagePickerController', '~> 3.5.8'
  pod 'TZImagePickerController', '~> 3.6.6'
  pod 'UITextView+Placeholder', '~> 1.4.0'
  pod 'YCShadowView', '~> 1.2.0'
  # pod 'GDPerformanceView', '~> 1.3.1'
  pod 'Bugly', '~> 2.5.90', :configurations => ['Debug']
  pod 'SocketRocket', '~> 0.6.0'
  # 使用Starscream断开连接没有走回调方法
  # pod 'Starscream', '~> 4.0.4'
  # pod 'MLeaksFinder', '~> 1.0.0', :configurations => ['Debug']
end

def tx_pods
  # 打包问题 https://cloud.tencent.com/document/product/269/32483
  pod 'Toast', '~> 4.0.0'

end

target 'OFWeekLive' do
  common_pods_all
  tx_pods
  target 'OFWeekLiveTests' do
    inherit! :search_paths
    # Pods for testing
  end
  
  target 'OFWeekLiveUITests' do
    # Pods for testing
  end
  
end

# 第三方框架 deployment target
post_install do |installer|
  installer.pods_project.build_configurations.each do |config|
        config.build_settings["EXCLUDED_ARCHS[sdk=iphonesimulator*]"] = "arm64"
        ## Fix for XCode 12.5 报错问题
        find_and_replace("Pods/FBRetainCycleDetector/FBRetainCycleDetector/Layout/Classes/FBClassStrongLayout.mm",
                         "layoutCache[currentClass] = ivars;", "layoutCache[(id<NSCopying>)currentClass] = ivars;")
        find_and_replace("Pods/FBRetainCycleDetector/fishhook/fishhook.c",
                         "indirect_symbol_bindings[i] = cur->rebindings[j].replacement;", "if (i < (sizeof(indirect_symbol_bindings) / sizeof(indirect_symbol_bindings[0]))) { \n indirect_symbol_bindings[i]=cur->rebindings[j].replacement; \n }")
      end
  # 警告的问题
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      if config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'].to_f <= 8.0
        config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '9.0'
      end
    end
  end
end

def find_and_replace(dir, findstr, replacestr)
  Dir[dir].each do |name|
      text = File.read(name)
      replace = text.gsub(findstr,replacestr)
      if text != replace
          puts "Fix: " + name
          File.open(name, "w") { |file| file.puts replace }
          STDOUT.flush
      end
  end
  Dir[dir + '*/'].each(&method(:find_and_replace))
end


# 关于 libwebp 导入问题
# 使用终端 open ~/.cocoapods/repos/master/Specs/1/9/2/libwebp
# 在对应的版本替换git地址：https://chromium.googlesource.com/webm/libwebp 替换为 https://github.com/webmproject/libwebp.git
