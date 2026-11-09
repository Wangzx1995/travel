# travel
旅行攻略
echo "1) 系统代理设置"; scutil --proxy | grep -E "Enable|Proxy|Port"
echo "2) 本地代理端口"; for p in 7890 7891 7897 7898 1080 1087 6152 6153 9090; do nc -z -G 1 127.0.0.1 $p 2>/dev/null && echo "  端口 $p 开放"; done
PX=$(scutil --proxy | awk '/HTTPSProxy/{p=$3} /HTTPSPort/{o=$3} END{if(p!="")printf "%s:%s",p,o}')
echo "3) 检测到代理: ${PX:-无}"
echo "4) 连通性测试"
for u in https://www.baidu.com https://www.google.com https://accounts.google.com https://www.gstatic.com/; do
  d=$(curl -s -o /dev/null -w "%{http_code}" --max-time 6 --noproxy '*' "$u" 2>/dev/null)
  p=""; [ -n "$PX" ] && p=$(curl -s -o /dev/null -w "%{http_code}" --max-time 8 -x "http://$PX" "$u" 2>/dev/null)
  echo "  $u  直连=${d:-超时}  走代理=${p:-超时}"
done
echo "5) Chrome 版本"; defaults read "/Applications/Google Chrome.app/Contents/Info.plist" CFBundleShortVersionString 2>/dev/null
echo "6) Chrome 已运行"; ps -o etime= -p $(pgrep -f "MacOS/Google Chrome$" | head -1) 2>/dev/null



1) 系统代理设置
2) 本地代理端口
3) 检测到代理: 无
4) 连通性测试
  https://www.baidu.com  直连=200  走代理=超时
  https://www.google.com  直连=000  走代理=超时
  https://accounts.google.com  直连=000  走代理=超时
  https://www.gstatic.com/  直连=000  走代理=超时
5) Chrome 版本
157.0.8103.38
6) Chrome 已运行
01:52:10
