<script>
  import ProgressBar from "progressbar.js";
  let progressBar;

  export function reset(milliseconds) {
    progressBar.set(1);
    progressBar.animate(0, {
      duration: milliseconds,
    });
  }

  const progressBarStart = (domNode) => {
    progressBar = new ProgressBar.Circle(domNode, {
      strokeWidth: 6,
    });
  };
</script>

<div use:progressBarStart />

<style>
  div {
    height: 20px;
    width: 20px;
    display: inline-block;
  }
</style>
