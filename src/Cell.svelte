<script>
  export let cell;
  export let onReveal;
  export let onFlag;
</script>

<style>
  .cell {
    display: block;
    width: 20px;
    height: 20px;
    line-height: 20px;
    border: 1px solid #aaa;
    text-align: center;
    background-color: #e0e0e0;
    cursor: pointer;
  }
  .cell:hover {
    background-color: #ededed;
  }
  .revealed {
    background-color: transparent;
    cursor: default;
  }
  .revealed:hover {
    background-color: transparent;
  }
  .mine.revealed {
    background-color: #ff6969;
  }
</style>

<div
  class="cell {cell.isMine ? 'mine' : ''}
  {cell.isRevealed ? 'revealed' : ''}"
  on:click={e => {
    onReveal(cell.idx);
  }}
  on:mousedown={e => {
    e.stopPropagation();
    if (e.button === 2) {
      onFlag(cell.idx);
    }
    return false;
  }}>
  {#if cell.isMine && cell.isRevealed}
    ✴
  {:else if cell.isFlagged}
    ⚑
  {:else if cell.neighbouringMinesCnt > 0 && cell.isRevealed}
    {cell.neighbouringMinesCnt}
  {/if}
</div>
