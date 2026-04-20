<script setup lang="ts">
import { onMounted, useTemplateRef } from 'vue'
import QRCode from 'qrcode'
import '../styles/download.css'

const userAgent = navigator.userAgent

const qrcodeCanvas = useTemplateRef<HTMLCanvasElement>('qrcodeCanvas')

function download(): void {
  if (window.location.href.includes('agent')) {
    alert(userAgent)
    return
  }

  if (/MicroMessenger/i.test(userAgent)) {
    alert('微信内不能下载，请点右上角使用浏览器打开！')
    return
  }

  if (/QQ/i.test(userAgent) && /iphone|ipod|ipad/i.test(userAgent)) {
    alert('QQ内不能下载，请使用其他浏览器打开！')
    return
  }

  if (!window.location.href.includes('android') && /Windows/i.test(userAgent)) {
    window.location.href = 'https://1674689971-1330588703.cos.ap-hongkong.myqcloud.com/game.zip'
    return
  }

  window.location.href = '/game.apk'
}

onMounted(() => {
  const canvas = qrcodeCanvas.value
  if (!canvas) {
    return
  }

  const qrTargetUrl = window.location.href
  QRCode.toCanvas(
    canvas,
    qrTargetUrl,
    {
      width: 160,
      margin: 1,
    },
    (error) => {
      if (error) {
        console.error('二维码生成失败：', error)
      }
    },
  )
})
</script>

<template>
  <div class="download-page combo">
    <div class="bg-glow bg-glow-1" />
    <div class="bg-glow bg-glow-2" />
    <main
      class="layout"
      itemscope
      itemtype="http://schema.org/SoftwareApplication"
    >
      <section class="hero">
        <p class="tag">
          OFFICIAL DOWNLOAD
        </p>
        <h1 class="title">
          客户端下载中心
        </h1>
        <p class="lead">
          稳定、安全、快速的官方版本下载入口。
        </p>
      </section>

      <section class="download-panel">
        <div class="app-card">
          <img
            class="app-icon"
            src="/app-icon.png"
            itemprop="image"
            alt="570游戏中心"
          >
          <div class="meta">
            <h2>570游戏中心</h2>
            <p class="version" itemprop="softwareVersion">
              v1.06 (Build 106) · 65.8 MB
            </p>
            <p class="date">
              更新时间：<span itemprop="datePublished">2026-04-11 11:08</span>
            </p>
          </div>
        </div>

        <div id="actions" class="action-card">
          <button type="button" @click="download">
            立即下载安装
          </button>
          <p>若在微信或 QQ 内打开，请使用右上角菜单在浏览器中继续下载。</p>
          <div class="qr-wrap">
            <canvas
              id="qrcodeCanvas"
              ref="qrcodeCanvas"
              width="160"
              height="160"
              aria-label="QR Code"
            />
            <span>扫码直达下载链接</span>
          </div>
        </div>
      </section>

      <section class="info-grid">
        <article class="block steps">
          <h3>安装步骤</h3>
          <ol>
            <li>点击“立即下载安装”下载最新安装包。</li>
            <li>Android 用户按提示允许安装未知来源应用。</li>
            <li>安装完成后，打开客户端并登录开始使用。</li>
          </ol>
        </article>
      </section>

      <section class="faq block">
        <h3>常见问题</h3>
        <div class="faq-item">
          <h4>下载按钮没反应怎么办？</h4>
          <p>请确认是否在微信或 QQ 内打开，建议复制链接到系统浏览器后重试。</p>
        </div>
        <div class="faq-item">
          <h4>Windows 用户下载到哪里？</h4>
          <p>Windows 端会自动下载压缩包文件，你可以解压后运行应用程序。</p>
        </div>
      </section>

      <section class="badge-row">
        <span class="badge">官方构建</span>
        <span class="badge">高速分发</span>
        <span class="badge">稳定更新</span>
        <span class="badge">多端可用</span>
      </section>

      <footer class="footer">
        <p>Copyright © 2026 Game570. All rights reserved.</p>
      </footer>
    </main>
  </div>
</template>
