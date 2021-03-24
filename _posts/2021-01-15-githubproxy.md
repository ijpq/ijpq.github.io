---
layout: post
title: acclerate github via proxy
comments: true
toc: true
tags: [github, proxy, accelerate]
---

<p>This tuturial will explain how to use git through a proxy, for example if you are
behind a firewall or on a private network.</p>

<p>The examples are valid for connections inside the .cms network at Point 5, but it
should be simple to adapt them to other configurations.</p>

<h3 id="what-kind-of-proxy">What kind of proxy</h3>

<p>The most common are an HTTP proxy, and a SOCKS5 proxy - for example, one opened with
the ssh -D command, documented in <a href="http://www.openbsd.org/cgi-bin/man.cgi?query=ssh&amp;sektion=1">ssh(1)</a>.</p>

<h3 id="how-to-open-a-socks-proxy-through-an-ssh-tunnel">How to open a SOCKS proxy through an SSH tunnel</h3>

<p>The <a href="http://www.openssh.com/">ssh command</a> distributed with most Unix-like systems
can open a SOCKS proxy on the local machine and forward all connections through the
ssh tunnel.</p>

<p>For example</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>ssh -f -N -D 1080 cmsusr.cms
</code></pre></div></div>

<p>will connect to the host <code class="language-plaintext highlighter-rouge">cmsusr.cms</code>, open a SOCKS proxy on the local host on
port 1080 (<code class="language-plaintext highlighter-rouge">-D 1080</code>), and put the process in background after a successful
connection (<code class="language-plaintext highlighter-rouge">-f -N</code>).</p>

<h3 id="how-to-connect-to-a-git-repository-using-the-ssh-protocol">How to connect to a git repository using the SSH protocol</h3>

<p>If the remote has a format like</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>git@github.com:cms-sw/cmssw.git
ssh://git@github.com/cms-sw/cmssw.git
</code></pre></div></div>

<p>then you are connecting to the git server using the <a href="http://git-scm.com/book/en/Git-on-the-Server-The-Protocols#The-SSH-Protocol">SSH protocol</a>.</p>

<p>In this case, git relis on ssh to handle the connection; in order to connect
through a SOCKS proxy you have to configure ssh itself, setting the <code class="language-plaintext highlighter-rouge">ProxyCommand</code>
option in your ~/.ssh/config file:</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>Host github.com
    User                    git
    ProxyCommand            nc -x localhost:1080 %h %p
</code></pre></div></div>

<p>OR On CentOS7 you can</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>Host github.com
    User                    git
    ProxyCommand            ssh cmsusr nc %h %p
</code></pre></div></div>

<p>For more information, see <a href="http://www.openbsd.org/cgi-bin/man.cgi?query=ssh_config&amp;sektion=5&amp;manpath=OpenBSD+Current&amp;arch=amd64&amp;format=html">ssh_config(5)</a>.</p>

<h3 id="how-to-connect-to-a-git-repository-using-the-http-or-https-protocols">How to connect to a git repository using the HTTP or HTTPS protocols</h3>

<p>If the remote has a format like</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>http://github.com/cms-sw/cmssw.git
https://github.com/cms-sw/cmssw.git
</code></pre></div></div>

<p>then you are connecting to the git server using the HTTP or HTTPS protocols.
In the old days, this used to be a <a href="http://git-scm.com/book/en/Git-on-the-Server-The-Protocols#The-HTTP/S-Protocol">dumb protocol</a>,
but since git 1.6.6 it uses a <a href="http://git-scm.com/blog/2010/03/04/smart-http.html">smart protocol</a>
similar to that used by SSH or GIT.</p>

<p>In this case git uses libcurl to handle the connection; the version of git bundled
with CMSSW supports different kinds of proxies: SOCKS4, SOCKS4a, SOCKS5, and HTTP/HTTPS.
In order to connect through any proxy supported by libcurl, you can set the <code class="language-plaintext highlighter-rouge">http.proxy</code> 
option:</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>git config --global http.proxy socks5://localhost:1080
</code></pre></div></div>

<p>For more information, see the <code class="language-plaintext highlighter-rouge">--proxy</code> option in <a href="http://curl.haxx.se/docs/manpage.html">curl(1)</a>
and the <code class="language-plaintext highlighter-rouge">http.proxy</code> entry in <a href="https://www.kernel.org/pub/software/scm/git/docs/git-config.html">git-config(1)</a>.</p>

<h3 id="how-to-connect-to-a-git-repository-using-the-git-protocol">How to connect to a git repository using the GIT protocol</h3>

<p>If the remote has a format like</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>git://github.com/cms-sw/cmssw.git
</code></pre></div></div>

<p>then you are connecting to the git server using the <a href="git-scm.com/book/en/Git-on-the-Server-The-Protocols#The-Git-Protocol">GIT protocol</a>.</p>

<p>In this case, it is possible to use a helper command to connect through any kind of proxy.
A simple script is included with CMSSW, to connect through a SOCKS5 proxy: <code class="language-plaintext highlighter-rouge">git-proxy</code>.
You can configure git to use it with</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>git config --global core.gitproxy "git-proxy"
git config --global socks.proxy "localhost:1080"
</code></pre></div></div>

<p>For more information, see <code class="language-plaintext highlighter-rouge">git-proxy --help</code>.</p>

[link](https://www.cnblogs.com/StevenWind/p/11735352.html)