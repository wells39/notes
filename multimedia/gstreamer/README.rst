========================================
GStreamer
========================================

* `基本介紹、設計概念 <introduction.rst>`_
* `利用 GStreamer 模擬常見指令 <gstreamer-based-common-commands.rst>`_
* `編譯 <build.rst>`_
* `GStreamer 相關 Dockerfile <dockerfile.rst>`_
* 內部溝通機制
* 靜態 Pipeline：Pipeline 執行後就再也不會更動
* 動態 Pipeline：Pipeline 會在執行過程中根據情況改變 Pipeline 流程
    - 重點是如何安全地跟執行中元件溝通並新增或刪除元件
    - Pad Probes：註冊 callback，當條件符合時會執行
    - Callback
    - Blocking Types
* 撰寫 Plugin
* Bus
* Pad Probe
* Signal
* Transcoding
* `Transmuxing <transmuxing.rst>`_
* Transrating
* Transsizing
* Adaptive Streaming
* File Repair
* 其他程式語言 Binding
    - gobject-introspection
    - typelibs
    - Python
        + PyGObject
        + gst-python
        + GStreamer + asyncio
        + Signal/Promise/Callback
        + Video Frame -> NumPy array
    - `Rust <lang-rust.rst>`_
* 除錯輔助
    - netsim - 模擬網路狀況
* `WebRTC </multimedia/webrtc/gstreamer-webrtc.rst>`_
* Server
* Filter
* `降噪 <noise-cancellation.rst>`_
* 即時
    - `分析 latency <latency.rst>`_
* `上字幕 (Subtitle) <subtitle.rst>`_
* `串流混合 (Mixing/Composition) <mixing.rst>`_
* `硬體加速 (Hardware Acceleration) </multimedia/hardware-acceleration/gstreamer-hardware-acceleration.rst>`_
* `藍牙 (Bluetooth) </multimedia/bluetooth/gstreamer-bluetooth.rst>`_
* `家用無線串流 </multimedia/home-wireless-streaming/gstreamer-home-wireless-streaming.rst>`_
* `播放器 (Player) <player.rst>`_
* `網頁顯示 (Web) <web.rst>`_
* `Cheatsheet <cheatsheet.rst>`_
* `GStreamer 相關公司 <company.rst>`_
* `Talks <talks.rst>`_
* 使用 GStreamer 的專案
    - Ardour
    - Pitivi
    - Cheese
    - GNOME Videos
    - Rhythmbox
    - Me TV
    - `Servo Media <https://github.com/servo/media>`_

* 版本追蹤 (雙號是穩定，單號是測試)
    - GStreamer 1.14
    - GStreamer 1.16

* Blogs
    - `Planet GStreamer <https://gstreamer.freedesktop.org/planet/>`_
    - `Nirbheek's Rantings <https://blog.nirbheek.in/>`_
    - `GStreamer – coaxion.net – slomo's blog <https://coaxion.net/blog/category/gstreamer/>`_
    - `GStreamer – Gkiagia's Blog <https://gkiagia.wordpress.com/category/gstreamer-2/>`_

* Conference
    - `GStreamer Conference <https://gstreamer.freedesktop.org/conference/>`_
        + `GStreamer Conference Videos <https://gstconf.ubicast.tv/>`_
    - FOSDEM
    - All Systems Go!
