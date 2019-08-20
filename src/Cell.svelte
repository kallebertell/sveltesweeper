<script>
export let cell;
export let onReveal;

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
}
.revealed {
  background-color: transparent;
}
.mine.revealed {
  background-color:#ff6969;
}
</style>

<div class="cell { cell.isMine ? 'mine' : ''} { cell.isRevealed ? 'revealed' : ''}" on:click={() => onReveal(cell.idx)}>
{#if cell.isMine && cell.isRevealed}*
{:else if cell.neighbouringMinesCnt > 0 && cell.isRevealed}{cell.neighbouringMinesCnt}
{/if} 
</div>
