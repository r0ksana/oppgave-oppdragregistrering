<script>
    //Setta opp variabla
    let oppdrag = [];
    let id;
    let Dato = null;
    let Namn = null;
    let Produkt = null;
    let Problem = null;
    let Feilmelding;
    let FeilmeldingStatus = false;
    let count = {
        "PC": 0,
        "mac": 0,
        "nettbrett": 0,
    };
    //Funksjon for å tella kvart produkt
    function calculateCount() {
        //setta tellingen til 0
        count = {
        "PC": 0,
        "mac": 0,
        "nettbrett": 0,
        }
        //Tella opp produkt
        oppdrag.forEach((item) => {
            if (count.hasOwnProperty(item.Produkt)) {
                count[item.Produkt] += 1;
            }
        });
    }
    

    // Legga til oppdrag
    function leggTil() {
        //Sjekka om da er data i kvar felt, viss da er da so fortsetter den. Viss da mangle, so går den til else
        if (Dato  != null && Namn != null && Produkt  != null && Problem != null){
            FeilmeldingStatus = false
        //Sjølve innsetinga
        oppdrag.push( 
            {
            "id": Math.round(Math.random() * 1000000),
            "Dato": Dato,
            "Namn": Namn,
            "Produkt": Produkt,
            "Problem": Problem
            }
        );
        //Sortera arrayen ette dato
        oppdrag = oppdrag.sort((a, b) => {
            return new Date(a.Dato) - new Date(b.Dato)
        })
        //Nullstilla felta/variablane slik at felta blir tømt og slik at sjekkinga av felt fungere rett.
        Dato = null;
        Namn = null;
        Produkt = null;
        Problem = null;
        calculateCount()
        //Kode som vise feilmelding viss eit eller fleire felt er tomme. Dan sjekker etter kva fellt som er tomme, og legger til i feilmeldinga kva det er som mangler.
        } else {
            FeilmeldingStatus = true
            Feilmelding = "Mangler " + " " +
            (Dato != null ? "" :  "dato,") + " " +
            (Namn != null ? "" :  "namn,") + " " +
            (Produkt != null ? "" :  "produkt,") + " " +
            (Problem != null ? "" :  "problem")
        }  
    }

    //Slette eit oppdrag
    function fjern (id) {
        oppdrag = oppdrag.filter(t => t.id != id)
        calculateCount()
        FeilmeldingStatus = false
    }

    //Fjerna alle oppdrag
    function fjernAlt() {
        oppdrag = []
        calculateCount()
        FeilmeldingStatus = false
    }


</script>
<table>
    <tr>
        <th>Dato</th>
        <th>Namn</th>
        <th>Produkt</th>
        <th>Problem</th>
        <!--Knapp for å fjerna alle oppdraga-->
        <th><button on:click={fjernAlt}>Slett alt</button></th>
    </tr>
    <!--Denne koden ser ette oppdrag, og for kvart oppdrag den finner so lager den ein til rad i tabellen--> 
    {#each oppdrag as {id, Dato, Namn, Produkt, Problem} }
        <tr>
            <td>{new Date(Dato).getDate()}/{new Date(Dato).getMonth()}/{new Date(Dato).getFullYear()}</td>
            <td>{Namn}</td>
            <td>{Produkt}</td>
            <td class="textarea">{Problem}</td>
            <!--Knapp for å fjerna innleget frå tabellen-->
            <td><button on:click={() => fjern(id)}>Fjern</button></td>
        </tr>
    {/each}
    <tr class="input">
        <td>
            <input name="Dato" bind:value={Dato} type="date">
        </td>   
        <td>
            <input name="Namn" bind:value={Namn} type="text">
        </td>
        <td>
            <select bind:value={Produkt}>
                <option value="PC">PC</option>
                <option value="mac">Mac</option>
                <option value="nettbrett">Nettbrett</option>
            </select>
        </td>
        <td>
            <textarea class="textarea" name="Problem" bind:value={Problem} rows="6"></textarea>
        </td> 
        <td>
            <!--Knapp for å senda inn verdiane lagt inn i felta-->
            <button on:click={leggTil}>Legg til</button>
        </td>
    </tr>
</table>
<div class="contentbox">
    <div class="box3">
        <!--Telle oppdrag-->
        <p style="padding-left:15px">Mengde oppdrag: {oppdrag.length}</p>
        <div class="chart-container">
        <!--Lage til søyle for kvar produkt, med lengde basert på mengde av produkte i arrayen-->
        {#each Object.entries(count) as [Produkt, count]}
        <div class="bar" style="height: {count / oppdrag.length * 90}%;">
        <span class="label">{Produkt} ({count})</span>
        </div>
        {/each}
        </div>
    </div>
    <div class="box4">
        <!--Sjekkea om det skal vises ei feilmelding, og viser/gøyma den-->
        {#if FeilmeldingStatus == true} 
            <div class="feilmeldingboks">
                <p class="feilmelding">{Feilmelding}</p>
            </div>
        {/if}
    </div>
</div>