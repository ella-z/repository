# canvas实现标签
- 使用canvas实现一个标签的效果
- html区域：
  ```
    <canvas id="canvas"> 您的浏览器不支持canvas，请升级您的浏览器 </canvas>
  ```
- JS区域：
  ```
    textSize(fontSize, fontFamily, text) {
      let span = document.createElement("span");
      let result = {};
      result.width = span.offsetWidth;
      result.height = span.offsetHeight;
      span.style.fontSize = fontSize;
      span.style.fontFamily = fontFamily;
      span.style.display = "inline-block";
      document.body.appendChild(span);
      if (typeof span.textContent != "undefined") {
        span.textContent = text;
      } else {
        span.innerText = text;
      }
      result.width = parseFloat(window.getComputedStyle(span).width) - result.width;
      result.height = parseFloat(window.getComputedStyle(span).height) - result.height;
      document.body.removeChild(span);
      return result;
    },
    drawRectLabel(context, x, y, text) {
      let textStyle = this.textSize("14px", "微软雅黑", text);
      let w = parseInt(textStyle.width) + 60;
      let h = 32;
      context.strokeStyle = "#5B9BD5"; //边框颜色
      context.strokeRect(x - w / 2, y - h / 2, w, h);
      context.fillStyle = "white"; //背景颜色
      context.fillRect(x - w / 2, y - h / 2, w, h);
      context.fillStyle = "#5B9BD5"; //文字颜色
      context.textAlign = "center";
      context.font = "normal 14px 微软雅黑";
      if (text) {
        context.fillText(text, x, y + (h - textStyle.height - 5) / 2);
      }
    },
    canvasInit() {
      const canvas = document.getElementById("canvas");
      //自适应宽高
      canvas.setAttribute("width", canvas.parentNode.clientWidth);
      canvas.setAttribute("height", canvas.parentNode.clientHeight);
      if (!canvas.getContext) {
        this.$alert("您的浏览器不支持canvas，请升级您的浏览器", {
          confirmButtonText: "确定",
        });
        return;
      }
      let ctx = canvas.getContext("2d");
      this.drawRectLabel(ctx, 100, 50, "db1.table1");
    }
  ```
