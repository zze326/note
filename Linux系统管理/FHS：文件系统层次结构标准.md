
<table class="wikitable" style="vertical-align: top; width: 100%" summary="一个层次结构的描述，FHS标准规定。">

<tbody><tr>
<th>目录
</th>
<th>描述
</th></tr>
<tr>
<td><tt>/</tt>
</td>
<td><i>第一层次结构</i> 的根、 整个文件系统层次结构的<a href="/wiki/%E6%A0%B9%E7%9B%AE%E5%BD%95" title="根目录">根目录</a>。
</td></tr>
<tr>
<td><tt>/bin/</tt>
</td>
<td>需要在<a href="/wiki/%E5%96%AE%E7%94%A8%E6%88%B6%E6%A8%A1%E5%BC%8F" title="单用户模式">单用户模式</a>可用的必要命令（<a href="/wiki/%E5%8F%AF%E6%89%A7%E8%A1%8C%E6%96%87%E4%BB%B6" class="mw-redirect" title="可执行文件">可执行文件</a>）；面向所有用户，<i>例如</i>： <a href="/wiki/Cat_(Unix)" title="Cat (Unix)">cat</a>、 <a href="/wiki/Ls" title="Ls">ls</a>、 <a href="/wiki/Cp_(Unix)" title="Cp (Unix)">cp</a>。
</td></tr>
<tr>
<td><tt>/boot/</tt>
</td>
<td><a href="/wiki/%E5%BC%95%E5%AF%BC%E7%A8%8B%E5%BA%8F" class="mw-redirect" title="引导程序">引导程序</a>文件，<i>例如：</i> <a href="/wiki/%E5%86%85%E6%A0%B8" title="内核">kernel</a>、<a href="/wiki/Initrd" title="Initrd">initrd</a>；时常是一个单独的分区<sup id="cite_ref-8" class="reference"><a href="#cite_note-8">[8]</a></sup>
</td></tr>
<tr>
<td><tt>/dev/</tt>
</td>
<td>必要<a href="/wiki/%E8%AE%BE%E5%A4%87%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F" title="设备文件系统">设备</a>, <i>例如：</i><tt><a href="/wiki//dev/null" title="/dev/null">/dev/null</a></tt>.
</td></tr>
<tr>
<td><tt>/etc/</tt>
</td>
<td>特定主机，系统范围内的<a href="/wiki/%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6" title="配置文件">配置文件</a>。
<p>关于这个名称目前有争议。在贝尔实验室关于UNIX实现文档的早期版本中，/etc 被称为<i><a href="/wiki/%E7%AD%89%E7%AD%89" title="等等">etcetera</a></i>，
<sup id="cite_ref-9" class="reference"><a href="#cite_note-9">[9]</a></sup>
这是由于过去此目录中存放所有不属于别处的所有东西（然而，FHS限制/etc只能存放静态配置文件，不能包含二进制文件）。
<sup id="cite_ref-10" class="reference"><a href="#cite_note-10">[10]</a></sup>
自从早期文档出版以来，目录名称已被以各种方式重新称呼。最近的解释包括<a href="/wiki/%E9%80%86%E5%90%91%E9%A6%96%E5%AD%97%E6%AF%8D%E7%BC%A9%E7%95%A5%E8%AF%8D" title="逆向首字母缩略词">逆向首字母缩略词</a>如："可编辑的文本配置"（英文 "Editable Text Configuration"）或"扩展工具箱"（英文 "Extended Tool Chest"）。
<sup id="cite_ref-11" class="reference"><a href="#cite_note-11">[11]</a></sup>
</p>
</td></tr>
<tr>
<td>
<dl><dd><tt>/etc/opt/</tt></dd></dl>
</td>
<td><tt>/opt/</tt>的配置文件
</td></tr>
<tr>
<td>
<dl><dd><tt>/etc/X11/</tt></dd></dl>
</td>
<td><a href="/wiki/X_Window%E7%B3%BB%E7%BB%9F" class="mw-redirect" title="X窗口系统">X窗口系统</a>(版本11)的配置文件
</td></tr>
<tr>
<td>
<dl><dd><tt>/etc/sgml/</tt></dd></dl>
</td>
<td><a href="/wiki/SGML" title="SGML">SGML</a>的配置文件
</td></tr>
<tr>
<td>
<dl><dd><tt>/etc/xml/</tt></dd></dl>
</td>
<td><a href="/wiki/XML" title="XML">XML</a>的配置文件
</td></tr>
<tr>
<td><tt>/home/</tt>
</td>
<td>用户的<a href="/wiki/%E5%AE%B6%E7%9B%AE%E5%BD%95" title="家目录">家目录</a>，包含保存的文件、个人设置等，一般为单独的分区。
</td></tr>
<tr>
<td><tt>/lib/</tt>
</td>
<td><tt>/bin/</tt> 和 <tt>/sbin/</tt>中二进制文件必要的<a href="/wiki/%E5%87%BD%E5%BC%8F%E5%BA%AB" title="函式库">库</a>文件。
</td></tr>
<tr>
<td><tt>/media/</tt>
</td>
<td>可移除媒体(如<a href="/wiki/CD-ROM" title="CD-ROM">CD-ROM</a>)的挂载点 (在FHS-2.3中出现)。
</td></tr>
<tr>
<td><tt>/mnt/</tt>
</td>
<td>临时<a href="/wiki/%E6%8C%82%E8%BD%BD" title="挂载">挂载</a>的文件系统。
</td></tr>
<tr>
<td><tt>/opt/</tt>
</td>
<td>可选<a href="/wiki/%E5%BA%94%E7%94%A8%E8%BD%AF%E4%BB%B6" class="mw-redirect" title="应用软件">应用软件</a> <a href="/wiki/%E8%BD%AF%E4%BB%B6%E5%8C%85" title="软件包">包</a>。<sup id="cite_ref-12" class="reference"><a href="#cite_note-12">[12]</a></sup>
</td></tr>
<tr>
<td><tt>/proc/</tt>
</td>
<td>虚拟<a href="/wiki/%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F" title="文件系统">文件系统</a>，将<a href="/wiki/%E5%86%85%E6%A0%B8" title="内核">内核</a>与<a href="/wiki/%E8%BF%9B%E7%A8%8B" class="mw-redirect" title="进程">进程</a>状态归档为文本文件。<i>例如：</i>uptime、 network。在Linux中，对应<a href="/wiki/Procfs" title="Procfs">Procfs</a>格式挂载。
</td></tr>
<tr>
<td><tt>/root/</tt>
</td>
<td><a href="/wiki/%E8%B6%85%E7%BA%A7%E7%94%A8%E6%88%B7" title="超级用户">超级用户</a>的<a href="/wiki/%E5%AE%B6%E7%9B%AE%E5%BD%95" title="家目录">家目录</a>
</td></tr>
<tr>
<td><tt>/sbin/</tt>
</td>
<td>必要的系统二进制文件，<i>例如：</i> init、 ip、 mount。
</td></tr>
<tr>
<td><tt>/srv/</tt>
</td>
<td>站点的具体<a href="/wiki/%E6%95%B0%E6%8D%AE" title="数据">数据</a>，由系统提供。
</td></tr>
<tr>
<td><tt>/tmp/</tt>
</td>
<td>临时文件(参见 <tt>/var/tmp</tt>)，在系统重启时目录中文件不会被保留。
</td></tr>
<tr>
<td><tt>/usr/</tt>
</td>
<td>用于存储只读用户数据的<i>第二层次</i>； 包含绝大多数的(<a href="/wiki/%E5%A4%9A%E7%94%A8%E6%88%B7" title="多用户">多</a>)用户工具和应用程序<sup id="cite_ref-13" class="reference"><a href="#cite_note-13">[13]</a></sup>。
</td></tr>
<tr>
<td>
<dl><dd><tt>/usr/bin/</tt></dd></dl>
</td>
<td>非必要<a href="/wiki/%E5%8F%AF%E6%89%A7%E8%A1%8C%E6%96%87%E4%BB%B6" class="mw-redirect" title="可执行文件">可执行文件</a> (在<a href="/wiki/%E5%96%AE%E7%94%A8%E6%88%B6%E6%A8%A1%E5%BC%8F" title="单用户模式">单用户模式</a>中不需要)；面向所有用户。
</td></tr>
<tr>
<td>
<dl><dd><tt>/usr/include/</tt></dd></dl>
</td>
<td>标准<a href="/wiki/%E5%A4%B4%E6%96%87%E4%BB%B6" title="头文件">包含文件</a>。
</td></tr>
<tr>
<td>
<dl><dd><tt>/usr/lib/</tt></dd></dl>
</td>
<td><tt>/usr/bin/</tt>和<tt>/usr/sbin/</tt>中二进制文件的<a href="/wiki/%E5%BA%93" class="mw-redirect" title="库">库</a>。
</td></tr>
<tr>
<td>
<dl><dd><tt>/usr/sbin/</tt></dd></dl>
</td>
<td>非必要的系统二进制文件，<i>例如：</i>大量<a href="/wiki/%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1" class="mw-redirect" title="网络服务">网络服务</a>的<a href="/wiki/%E5%AE%88%E6%8A%A4%E8%BF%9B%E7%A8%8B" title="守护进程">守护进程</a>。
</td></tr>
<tr>
<td>
<dl><dd><tt>/usr/share/</tt></dd></dl>
</td>
<td><a href="/wiki/%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84" class="mw-redirect" title="体系结构">体系结构</a>无关（共享）数据。
</td></tr>
<tr>
<td>
<dl><dd><tt>/usr/src/</tt></dd></dl>
</td>
<td><a href="/wiki/%E6%BA%90%E4%BB%A3%E7%A0%81" title="源代码">源代码</a>,<i>例如:</i>内核源代码及其头文件。
</td></tr>
<tr>
<td>
<dl><dd><tt>/usr/X11R6/</tt></dd></dl>
</td>
<td><a href="/wiki/X_Window%E7%B3%BB%E7%BB%9F" class="mw-redirect" title="X窗口系统">X窗口系统</a> 版本 11, Release 6.
</td></tr>
<tr>
<td>
<dl><dd><tt>/usr/local/</tt></dd></dl>
</td>
<td>本地数据的<i>第三层次</i>， 具体到本台主机。通常而言有进一步的子目录， <i>例如：</i><tt>bin/</tt>、<tt>lib/</tt>、<tt>share/</tt>.
<p><sup id="cite_ref-14" class="reference"><a href="#cite_note-14">[14]</a></sup>
</p>
</td></tr>
<tr>
<td><tt>/var/</tt>
</td>
<td>变量文件——在正常运行的系统中其内容不断变化的文件，如日志，脱机文件和临时电子邮件文件。有时是一个单独的分区。
</td></tr>
<tr>
<td>
<dl><dd><tt>/var/cache/</tt></dd></dl>
</td>
<td>应用程序缓存数据。这些数据是在本地生成的一个耗时的I/O或计算结果。应用程序必须能够再生或恢复数据。缓存的文件可以被删除而不导致数据丢失。
</td></tr>
<tr>
<td>
<dl><dd><tt>/var/lib/</tt></dd></dl>
</td>
<td>状态信息。 由程序在运行时维护的持久性数据。 <i>例如：</i>数据库、包装的系统元数据等。
</td></tr>
<tr>
<td>
<dl><dd><tt>/var/lock/</tt></dd></dl>
</td>
<td>锁文件，一类跟踪当前使用中资源的文件。
</td></tr>
<tr>
<td>
<dl><dd><tt>/var/log/</tt></dd></dl>
</td>
<td>日志文件，包含大量日志文件，为了防止日志占满根分区，生产环境中一般是单独分区。
</td></tr>
<tr>
<td>
<dl><dd><tt>/var/mail/</tt></dd></dl>
</td>
<td>用户的<a href="/wiki/%E7%94%B5%E5%AD%90%E9%82%AE%E7%AE%B1" class="mw-redirect" title="电子邮箱">电子邮箱</a>。
</td></tr>
<tr>
<td>
<dl><dd><tt>/var/run/</tt></dd></dl>
</td>
<td>自最后一次启动以来运行中的系统的信息，<i>例如：</i>当前登录的用户和运行中的<a href="/wiki/%E5%AE%88%E6%8A%A4%E8%BF%9B%E7%A8%8B" title="守护进程">守护进程</a>、一些守护进程的pid文件、socket文件。现已经被/run代替<sup id="cite_ref-15" class="reference"><a href="#cite_note-15">[15]</a></sup>。
</td></tr>
<tr>
<td>
<dl><dd><tt>/var/spool/</tt></dd></dl>
</td>
<td>等待处理的任务的<a href="/w/index.php?title=%E8%84%B1%E6%9C%BA%E6%96%87%E4%BB%B6&amp;action=edit&amp;redlink=1" class="new" title="脱机文件（页面不存在）">脱机文件</a>，<i>例如：</i>打印队列和未读的邮件。
</td></tr>
<tr>
<td>
<dl><dd><tt>/var/spool/mail/</tt></dd></dl>
</td>
<td>用户的邮箱(不鼓励的存储位置)
</td></tr>
<tr>
<td>
<dl><dd><tt>/var/tmp/</tt></dd></dl>
</td>
<td>在系统重启过程中可以保留的临时文件。
</td></tr>
<tr>
<td><tt>/run/</tt>
</td>
<td>代替/var/run目录。
</td></tr>
</tbody></table>

> - <https://zh.wikipedia.org/zh-cn/%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84%E6%A0%87%E5%87%86>