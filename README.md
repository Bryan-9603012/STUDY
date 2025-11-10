<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
<meta name="theme-color" content="#2c3e50">
<title>Linux Fundamentals 筆記</title>
<style>
  body {
    font-family: "Noto Sans", Arial, sans-serif;
    background-color: #f4f4f4;
    color: #333;
    line-height: 1.7;
    margin: 0;
    padding: 20px;
  }
  h1 {
    text-align: center;
    margin-top: 10px;
    color: #2c3e50;
  }
  h2 {
    border-left: 5px solid #2c3e50;
    padding-left: 8px;
    margin-top: 28px;
    color: #2c3e50;
  }
  pre {
    background: #232629;
    color: #f8f8f2;
    padding: 10px;
    overflow-x: auto;
    border-radius: 5px;
    font-size: 0.9rem;
  }
  table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 15px;
    font-size: 0.9rem;
  }
  th, td {
    border: 1px solid #bbb;
    padding: 8px;
    text-align: left;
  }
  th {
    background: #34495e;
    color: #fff;
  }
  code {
    background: #eaeaea;
    padding: 2px 5px;
    border-radius: 3px;
    font-family: Consolas, monospace;
  }
  details summary {
    font-weight: bold;
    cursor: pointer;
    margin-top: 12px;
    color: #2c3e50;
  }
  section {
    margin-bottom: 30px;
  }
  ul {
    margin: 10px 0 10px 20px;
  }

  /* 手機調整 */
  @media (max-width: 700px) {
    body {
      padding: 15px;
      font-size: 16px;
    }
    pre {
      font-size: 0.85rem;
    }
    table, th, td {
      font-size: 0.85rem;
      display: block;
      width: 100%;
    }
    th {
      display: none;
    }
    td {
      border: none;
      border-bottom: 1px solid #ccc;
      padding: 6px 0;
    }
  }
</style>
</head>
<body>

<h1>Linux Fundamentals 筆記</h1>

<section>
<h2>1. 系統資訊 (System Information)</h2>
<table>
<caption>常用系統查詢指令</caption>
<tr><th>指令</th><th>功能</th></tr>
<tr><td>whoami / id</td><td>顯示目前使用者、群組資訊</td></tr>
<tr><td>hostname / uname</td><td>主機名稱、核心&硬體資訊</td></tr>
<tr><td>pwd</td><td>顯示目前工作目錄</td></tr>
<tr><td>ps</td><td>顯示程序狀態</td></tr>
<tr><td>lsblk / lsusb / lspci</td><td>磁碟、USB、PCI 硬體</td></tr>
<tr><td>env</td><td>環境變數查詢</td></tr>
<tr><td>ifconfig / ip / netstat / ss</td><td>網路資訊、狀態</td></tr>
</table>
<pre>
ssh htb-student@[IP address]
hostname
whoami
id
uname -a
uname -r
</pre>
</section>

<section>
<h2>2. 導航 (Navigation)</h2>
<pre>
pwd                   # 目前所在路徑
ls -l / -a /path      # 列目錄、含權限資訊與隱藏檔
cd /path  cd ..  cd - # 進目錄、回上一層、上次目錄
clear                 # 清除畫面 (或 Ctrl+L)
</pre>
<ul>
<li>自動補齊：<code>TAB</code></li>
<li>命令歷史查找：<code>↑ / ↓</code>、<code>Ctrl+R</code></li>
</ul>
</section>

<section>
<h2>3. 檔案與目錄操作 (Files & Directories)</h2>
<pre>
touch 檔案            # 建空檔案
mkdir [-p] 目錄       # 建目錄，含多層(-p)
rm 檔案/ -r 目錄      # 刪除檔案/遞迴刪目錄
cp [-r] src dest      # 複製檔案/目錄
mv old new            # 移動、更名
tree .                # 樹狀目錄顯示
</pre>
<details>
<summary>檔案內容預覽與搜尋</summary>
<pre>
cat file             # 顯示所有內容
less/more file       # 分頁檢視
head/tail file       # 前/後數行
grep 'keyword' 檔案  # 關鍵字搜尋
find . -name '*.txt' # 檔案搜尋
</pre>
</details>
</section>

<section>
<h2>4. 權限管理與安全 (Permissions)</h2>
<table>
<caption>權限語法與操作</caption>
<tr><th>指令</th><th>用途/範例</th></tr>
<tr><td>ls -l</td><td>顯示權限 (-rw-r--r--)</td></tr>
<tr><td>chmod 755 file</td><td>設權限 (rwxr-xr-x)</td></tr>
<tr><td>chmod u+x,g-w,o-r file</td><td>符號法加/減權限</td></tr>
<tr><td>chown user:group file</td><td>改檔案擁有者/群組</td></tr>
<tr><td>find mydir/ -type f -exec chmod 644 {} ;</td><td>批量調整檔案權限</td></tr>
</table>
<ul>
<li>建議：目錄 755，檔案 644，重要檔案 600</li>
<li>/etc/passwd、/etc/shadow 需要保護權限</li>
</ul>
</section>

<section>
<h2>5. 編輯器入門 (Text Editors)</h2>
<table>
<caption>常見編輯器</caption>
<tr><th>編輯器</th><th>啟動指令</th><th>特色</th></tr>
<tr><td>nano</td><td><code>nano 檔案</code></td><td>最簡單、底部顯示快捷鍵</td></tr>
<tr><td>vim/vi</td><td><code>vim 檔案</code></td><td>高效、適進階使用者</td></tr>
<tr><td>vimtutor</td><td><code>vimtutor</code></td><td>官方練習教學</td></tr>
</table>
<pre>
# Nano 快捷
Ctrl+O 存檔  Ctrl+X 離開  Ctrl+W 搜尋

# Vim 常用
i 進入編輯  Esc 回普通模式  :w 存檔  :q 離開
:wq 存且離開  :q! 不保存強制離開
</pre>
</section>

<section>
<h2>6. 常用文字/資料流指令</h2>
<pre>
echo "文字" > 檔案        # 覆蓋寫檔案
echo "補充" >> 檔案       # 追加內容
cat file1 file2 > all.txt # 合併輸出
sort target.txt | uniq    # 排序並去重複
awk '{print $1}' file     # 取出每行第一欄
sed 's/old/new/g' file    # 取代內容
tee out.txt               # 輸出同時寫檔
</pre>
</section>

<section>
<h2>7. Terminal 熱鍵與高效技巧</h2>
<ul>
<li>中止程序：Ctrl+C，登出/EOF：Ctrl+D</li>
<li>清空行：Ctrl+U，清屏：Ctrl+L</li>
<li>上/下鍵查歷史、Tab 補齊、Ctrl+R 倒查命令</li>
<li>多窗口/分頁建議學 tmux 或 screen</li>
</ul>
</section>

</body>
</html>
