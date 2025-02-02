<script>
  import RedProgress from "./red-progress.svelte";

    let size = $state(50);
    let color = $state('#ff3e00');

    let canvas;

    $effect(() => {
        const ctx = canvas.getContext('2d');
        // 清空画布
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        // 设置填充颜色
        ctx.fillStyle = color;
        // 绘制外部圆并填充
        ctx.beginPath();
        ctx.arc(75, 75, 50, 0, Math.PI * 2, true);
        ctx.fill();
        // 绘制外部圆的边框
        ctx.beginPath();
        ctx.arc(75, 75, 50, 0, Math.PI * 2, true);
        ctx.stroke();
        // 绘制嘴巴和眼睛
        ctx.beginPath();
        ctx.moveTo(110, 75);
        ctx.arc(75, 75, 35, 0, Math.PI, false);
        ctx.stroke();
        ctx.beginPath();
        ctx.moveTo(65, 65);
        ctx.arc(60, 65, 5, 0, Math.PI * 2, true);
        ctx.stroke();
        ctx.beginPath();
        ctx.moveTo(95, 65);
        ctx.arc(90, 65, 5, 0, Math.PI * 2, true);
        ctx.stroke();
    });
</script>

<div>
  <canvas bind:this={canvas} width="200" height="200" ></canvas>
  <RedProgress />
</div>