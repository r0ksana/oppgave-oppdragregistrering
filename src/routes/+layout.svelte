<div class="font background">
    <div class="header headerfont">
        <div class="headerfont paddingright">
            Oppdragsregistrering
        </div>
    </div>
    <div class="stuffgohere">
        <div class="content-area stuffgohere">
            <div class="content-box">
                <div class="box1">
                    <slot><!-- Det som ligger i +page.svelte ligger inni her--></slot>
                </div>
            </div>
        </div>
    </div>
    <div class="footer">
        © 2024 IT-konsulenten
        <div class="footertext">Beste nettsida i verden.</div> 
    </div>
</div>

<style>
    @import 'global.css';
</style>