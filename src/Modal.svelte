<script>
import { blur,fly,fade } from 'svelte/transition';

</script>
<div class='modal-container' in:blur out:fade>
    <div class='modal-content' transition:fly={{y:200}}>
        <slot></slot>
   
    </div>
</div>