基本格式：
```bash
\e[风格;前景色;背景色m
```

或者使用 `\033` 代替 `\e`，因为 `\e` 是 `\033`（ASCII码27的八进制表示）的另一种写法。其中：
- `\e[` 或 `\033[` 是开始一个转义序列的控制字符。
- 风格是一个或多个以分号分隔的数字，用于指定文本属性，如 1 表示高亮（加粗），4表示下划线等。
- 前景色和背景色也是数字，分别表示文本颜色和背景颜色。
- `m` 结束颜色设置序列。

代码：
<table class="wikitable">
<tbody><tr>
<th>代码</th>
<th>作用</th>
<th>备注
</th></tr>
<tr>
<td>0</td>
<td>重置/正常</td>
<td>关闭所有属性。
</td></tr>
<tr>
<td>1</td>
<td>粗体或增加强度</td>
<td>
</td></tr>
<tr>
<td>2</td>
<td>弱化（降低强度）</td>
<td>未广泛支持。
</td></tr>
<tr>
<td>3</td>
<td>斜体</td>
<td>未广泛支持。有时视为反相显示。
</td></tr>
<tr>
<td>4</td>
<td>下划线</td>
<td>
</td></tr>
<tr>
<td>5</td>
<td>缓慢闪烁</td>
<td>低于每分钟150次。
</td></tr>
<tr>
<td>6</td>
<td>快速闪烁</td>
<td>MS-DOS ANSI.SYS；每分钟150以上；未广泛支持。
</td></tr>
<tr>
<td>7</td>
<td>反显</td>
<td>前景色与背景色交换。
</td></tr>
<tr>
<td>8</td>
<td>隐藏</td>
<td>未广泛支持。
</td></tr>
<tr>
<td>9</td>
<td>划除</td>
<td>字符清晰，但标记为删除。未广泛支持。
</td></tr>
<tr>
<td>10</td>
<td>主要（默认）字体</td>
<td>
</td></tr>
<tr>
<td>11–19</td>
<td>替代字体</td>
<td>选择替代字体<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="http://www.w3.org/1998/Math/MathML" alttext="{\displaystyle n-10}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>n</mi>
        <mo>−<!-- − --></mo>
        <mn>10</mn>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle n-10}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/f84f9418cc9f1de8428f11785ae95b5415b425c5" class="mwe-math-fallback-image-inline mw-invert skin-invert" aria-hidden="true" style="vertical-align: -0.505ex; width:6.56ex; height:2.343ex;" alt="{\displaystyle n-10}"></span>。
</td></tr>
<tr>
<td>20</td>
<td><a href="/wiki/%E5%BE%B7%E6%96%87%E5%B0%96%E8%A7%92%E9%AB%94" title="德文尖角体">尖角体</a></td>
<td>几乎无支持。
</td></tr>
<tr>
<td>21</td>
<td>关闭粗体或双下划线</td>
<td>关闭粗体未广泛支持；双下划线几乎无支持。
</td></tr>
<tr>
<td>22</td>
<td>正常颜色或强度</td>
<td>不强不弱。
</td></tr>
<tr>
<td>23</td>
<td>非斜体、非尖角体</td>
<td>
</td></tr>
<tr>
<td>24</td>
<td>关闭下划线</td>
<td>去掉单双下划线。
</td></tr>
<tr>
<td>25</td>
<td>关闭闪烁</td>
<td>
</td></tr>
<tr>
<td>27</td>
<td>关闭反显</td>
<td>
</td></tr>
<tr>
<td>28</td>
<td>关闭隐藏</td>
<td>
</td></tr>
<tr>
<td>29</td>
<td>关闭划除</td>
<td>
</td></tr>
<tr>
<td>30–37</td>
<td>设置前景色</td>
<td>参见下面的颜色表。
</td></tr>
<tr>
<td>38</td>
<td>设置前景色</td>
<td>下一个参数是<code>5;n</code>或<code>2;r;g;b</code>，见下。
</td></tr>
<tr>
<td>39</td>
<td>默认前景色</td>
<td>由具体实现定义（按照标准）。
</td></tr>
<tr>
<td>40–47</td>
<td>设置背景色</td>
<td>参见下面的颜色表。
</td></tr>
<tr>
<td>48</td>
<td>设置背景色</td>
<td>下一个参数是<code>5;n</code>或<code>2;r;g;b</code>，见下。
</td></tr>
<tr>
<td>49</td>
<td>默认背景色</td>
<td>由具体实现定义（按照标准）。
</td></tr>
<tr>
<td>51</td>
<td>Framed</td>
<td>
</td></tr>
<tr>
<td>52</td>
<td>Encircled</td>
<td>
</td></tr>
<tr>
<td>53</td>
<td>上划线</td>
<td>
</td></tr>
<tr>
<td>54</td>
<td>Not framed or encircled</td>
<td>
</td></tr>
<tr>
<td>55</td>
<td>关闭上划线</td>
<td>
</td></tr>
<tr>
<td>60</td>
<td>表意文字下划线或右边线</td>
<td rowspan="5">几乎无支持。
</td></tr>
<tr>
<td>61</td>
<td>表意文字双下划线或双右边线
</td></tr>
<tr>
<td>62</td>
<td>表意文字上划线或左边线
</td></tr>
<tr>
<td>63</td>
<td>表意文字双上划线或双左边线
</td></tr>
<tr>
<td>64</td>
<td>表意文字着重标志
</td></tr>
<tr>
<td>65</td>
<td>表意文字属性关闭</td>
<td>重置<code>60</code>–<code>64</code>的所有效果。
</td></tr>
<tr>
<td>90–97</td>
<td>设置明亮的前景色</td>
<td>aixterm（非标准）。
</td></tr>
<tr>
<td><span class="nowrap">100–107</span></td>
<td>设置明亮的背景色</td>
<td>aixterm（非标准）。
</td></tr></tbody></table>

颜色：
<table class="wikitable">

<tbody><tr>
<th>名称</th>
<th>前景色代码</th>
<th>背景色代码
</th>
<th>VGA<sup id="cite_ref-16" class="reference"><a href="#cite_note-16" title="">[nb 2]</a></sup>
</th>
<th><a href="/wiki/%E5%91%BD%E4%BB%A4%E6%8F%90%E7%A4%BA%E7%AC%A6" class="mw-redirect" title="命令提示符">CMD</a><sup id="cite_ref-17" class="reference"><a href="#cite_note-17">[nb 3]</a></sup>
</th>
<th><a href="/wiki/%E7%BB%88%E7%AB%AF_(macOS)" title="终端 (macOS)">Terminal.app</a>
</th>
<th><a href="/wiki/PuTTY" title="PuTTY">PuTTY</a>
</th>
<th><a href="/wiki/MIRC" title="MIRC">mIRC</a>
</th>
<th><a href="/wiki/Xterm" title="Xterm">xterm</a>
</th>
<th><span class="ilh-all" data-orig-title="X11颜色名称" data-lang-code="en" data-lang-name="英语" data-foreign-title="X11 color names"><span class="ilh-page"><a href="/w/index.php?title=X11%E9%A2%9C%E8%89%B2%E5%90%8D%E7%A7%B0&amp;action=edit&amp;redlink=1" class="new" original-title="X11颜色名称（页面不存在）">X</a></span><span class="noprint ilh-comment">（<span class="ilh-lang">英语</span><span class="ilh-colon">：</span><span class="ilh-link"><a href="https://en.wikipedia.org/wiki/X11_color_names" class="extiw" title="en:X11 color names"><span lang="en" dir="auto">X11 color names</span></a></span>）</span></span><sup id="cite_ref-18" class="reference"><a href="#cite_note-18" title="">[nb 4]</a></sup>
</th>
<th><a href="/wiki/Ubuntu" title="Ubuntu">Ubuntu</a><sup id="cite_ref-19" class="reference"><a href="#cite_note-19">[nb 5]</a></sup>
</th></tr>
<tr>
<td>黑</td>
<td>30</td>
<td>40
</td>
<td colspan="7" style="background: #000; color: white">0,0,0
</td>
<td style="background: #010101; color: white">1,1,1
</td></tr>
<tr>
<td>红</td>
<td>31</td>
<td>41
</td>
<td style="background: #AA0000; color: white">170,0,0
</td>
<td style="background: #800000; color: white">128,0,0
</td>
<td style="background: #c23621; color: white">194,54,33
</td>
<td style="background: #bb0000; color: white">187,0,0
</td>
<td style="background: #7f0000; color: white">127,0,0
</td>
<td style="background: #cd0000; color: white">205,0,0
</td>
<td style="background: #ff0000; color: white">255,0,0
</td>
<td style="background: #de382b; color: white">222,56,43
</td></tr>
<tr>
<td>绿</td>
<td>32</td>
<td>42
</td>
<td style="background: #00AA00; color: white">0,170,0
</td>
<td style="background: #008000; color: white">0,128,0
</td>
<td style="background: #25bc26; color: white">37,188,36
</td>
<td style="background: #00bb00; color: white">0,187,0
</td>
<td style="background: #009300; color: white">0,147,0
</td>
<td style="background: #00cd00; color: white">0,205,0
</td>
<td style="background: #00ff00; color: black">0,255,0
</td>
<td style="background: #39b54a; color: black">57,181,74
</td></tr>
<tr>
<td>黄</td>
<td>33</td>
<td>43
</td>
<td style="background: #AA5500; color: white">170,85,0<sup id="cite_ref-20" class="reference"><a href="#cite_note-20" title="">[nb 6]</a></sup>
</td>
<td style="background: #808000; color: white">128,128,0
</td>
<td style="background: #adad27; color: white">173,173,39
</td>
<td style="background: #bbbb00; color: black">187,187,0
</td>
<td style="background: #fc7f00; color: white">252,127,0
</td>
<td style="background: #cdcd00; color: black">205,205,0
</td>
<td style="background: #ffff00; color: black">255,255,0
</td>
<td style="background: #ffc706; color: black">255,199,6
</td></tr>
<tr>
<td>蓝</td>
<td>34</td>
<td>44
</td>
<td style="background: #0000AA; color: white">0,0,170
</td>
<td style="background: #000080; color: white">0,0,128
</td>
<td style="background: #492ee1; color: white">73,46,225
</td>
<td style="background: #0000bb; color: white">0,0,187
</td>
<td style="background: #00007f; color: white">0,0,127
</td>
<td style="background: #0000ee; color: white">0,0,238<sup id="cite_ref-21" class="reference"><a href="#cite_note-21">[15]</a></sup>
</td>
<td style="background: #0000ff; color: white">0,0,255
</td>
<td style="background: #006fb8; color: white">0,111,184
</td></tr>
<tr>
<td>品红</td>
<td>35</td>
<td>45
</td>
<td style="background: #AA00AA; color: white">170,0,170
</td>
<td style="background: #800080; color: white">128,0,128
</td>
<td style="background: #d338d3; color: white">211,56,211
</td>
<td style="background: #bb00bb; color: white">187,0,187
</td>
<td style="background: #9c009c; color: white">156,0,156
</td>
<td style="background: #cd00cd; color: white">205,0,205
</td>
<td style="background: #ff00ff; color: black">255,0,255
</td>
<td style="background: #762671; color: white">118,38,113
</td></tr>
<tr>
<td>青</td>
<td>36</td>
<td>46
</td>
<td style="background: #00AAAA; color: white">0,170,170
</td>
<td style="background: #008080; color: white">0,128,128
</td>
<td style="background: #33bbc8; color: white">51,187,200
</td>
<td style="background: #00bbbb; color: black">0,187,187
</td>
<td style="background: #009393; color: white">0,147,147
</td>
<td style="background: #00cdcd; color: black">0,205,205
</td>
<td style="background: #00ffff; color: black">0,255,255
</td>
<td style="background: #2cb5e9; color: black">44,181,233
</td></tr>
<tr>
<td>白</td>
<td>37</td>
<td>47
</td>
<td style="background: #AAAAAA; color: black">170,170,170
</td>
<td style="background: #c0c0c0; color: black">192,192,192
</td>
<td style="background: #cbcccd; color: black">203,204,205
</td>
<td style="background: #bbbbbb; color: black">187,187,187
</td>
<td style="background: #d2d2d2; color: black">210,210,210
</td>
<td style="background: #e5e5e5; color: black">229,229,229
</td>
<td style="background: #ffffff; color: black">255,255,255
</td>
<td style="background: #cccccc; color: black">204,204,204
</td></tr>
<tr>
<td>亮黑（灰）</td>
<td>90</td>
<td>100
</td>
<td style="background: #555555; color: white">85,85,85
</td>
<td style="background: #808080; color: black">128,128,128
</td>
<td style="background: #818383; color: black">129,131,131
</td>
<td style="background: #555555; color: white">85,85,85
</td>
<td style="background: #7f7f7f; color: white">127,127,127
</td>
<td style="background: #7f7f7f; color: white">127,127,127
</td>
<td>
</td>
<td style="background: #808080; color: black">128,128,128
</td></tr>
<tr>
<td>亮红</td>
<td>91</td>
<td>101
</td>
<td style="background: #FF5555; color: white">255,85,85
</td>
<td style="background: #ff0000; color: white">255,0,0
</td>
<td style="background: #fc391f; color: white">252,57,31
</td>
<td style="background: #ff5555; color: white">255,85,85
</td>
<td style="background: #ff0000; color: white">255,0,0
</td>
<td style="background: #ff0000; color: white">255,0,0
</td>
<td>
</td>
<td style="background: #ff0000; color: white">255,0,0
</td></tr>
<tr>
<td>亮绿</td>
<td>92</td>
<td>102
</td>
<td style="background: #55FF55; color: black">85,255,85
</td>
<td style="background: #00ff00; color: black">0,255,0
</td>
<td style="background: #31e722; color: black">49,231,34
</td>
<td style="background: #55ff55; color: black">85,255,85
</td>
<td style="background: #00fc00; color: black">0,252,0
</td>
<td style="background: #00ff00; color: black">0,255,0
</td>
<td style="background: #90ee90; color: black">144,238,144
</td>
<td style="background: #00ff00; color: black">0,255,0
</td></tr>
<tr>
<td>亮黄</td>
<td>93</td>
<td>103
</td>
<td style="background: #FFFF55; color: black">255,255,85
</td>
<td style="background: #ffff00; color: black">255,255,0
</td>
<td style="background: #eaec23; color: black">234,236,35
</td>
<td style="background: #ffff55; color: black">255,255,85
</td>
<td style="background: #ffff00; color: black">255,255,0
</td>
<td style="background: #ffff00; color: black">255,255,0
</td>
<td style="background: #ffffe0; color: black">255,255,224
</td>
<td style="background: #ffff00; color: black">255,255,0
</td></tr>
<tr>
<td>亮蓝</td>
<td>94</td>
<td>104
</td>
<td style="background: #5555FF; color: white">85,85,255
</td>
<td style="background: #0000ff; color: white">0,0,255
</td>
<td style="background: #5833ff; color: black">88,51,255
</td>
<td style="background: #5555ff; color: black">85,85,255
</td>
<td style="background: #0000fc; color: white">0,0,252
</td>
<td style="background: #5c5cff; color: black">92,92,255<sup id="cite_ref-22" class="reference"><a href="#cite_note-22">[16]</a></sup>
</td>
<td style="background: #acd8e6; color: black">173,216,230
</td>
<td style="background: #0000ff; color: white">0,0,255
</td></tr>
<tr>
<td>亮品红</td>
<td>95</td>
<td>105
</td>
<td style="background: #FF55FF; color: black">255,85,255
</td>
<td style="background: #ff00ff; color: white">255,0,255
</td>
<td style="background: #f935f8; color: black">249,53,248
</td>
<td style="background: #ff55ff; color: black">255,85,255
</td>
<td style="background: #ff00ff; color: white">255,0,255
</td>
<td style="background: #ff00ff; color: white">255,0,255
</td>
<td>
</td>
<td style="background: #ff00ff; color: white">255,0,255
</td></tr>
<tr>
<td>亮青</td>
<td>96</td>
<td>106
</td>
<td style="background: #55FFFF; color: black">85,255,255
</td>
<td style="background: #00ffff; color: black">0,255,255
</td>
<td style="background: #14f0f0; color: black">20,240,240
</td>
<td style="background: #55ffff; color: black">85,255,255
</td>
<td style="background: #00ffff; color: black">0,255,255
</td>
<td style="background: #00ffff; color: black">0,255,255
</td>
<td style="background: #e0ffff; color: black">224,255,255
</td>
<td style="background: #00ffff; color: black">0,255,255
</td></tr>
<tr>
<td>亮白</td>
<td>97</td>
<td>107
</td>
<td style="background: #FFFFFF; color: black">255,255,255
</td>
<td style="background: #ffffff; color: black">255,255,255
</td>
<td style="background: #e9ebeb; color: black">233,235,235
</td>
<td style="background: #ffffff; color: black">255,255,255
</td>
<td style="background: #ffffff; color: black">255,255,255
</td>
<td style="background: #ffffff; color: black">255,255,255
</td>
<td>
</td>
<td style="background: #ffffff; color: black">255,255,255
</td></tr></tbody></table>

> - <https://zh.wikipedia.org/wiki/ANSI%E8%BD%AC%E4%B9%89%E5%BA%8F%E5%88%97>