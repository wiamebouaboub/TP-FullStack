<template>
    <div>
        
        <select id="select" v-on:load="printVilles($event)" @change="printVilles($event)">
            
        </select>
        <div id="tab" style="display: none;">
            <h2 id="titre">Liste des villes:  </h2>
            <table class="table" id="tableau">
            <tr><th scope="col">Nom</th><th scope="col">Population</th> <th scope="col">Supprimé</th></tr>
            
            <VilleItem v-for="(ville,index) in hookVille" 
                :index="index"
                :ville="ville"
                :key="index"
                @eventSup="deleteVille"/>
                
        
            
            </table>
        </div>
    </div>
</template>
<script setup>
            import VilleItem from '../components/VilleItem.vue'
            import { onMounted, reactive } from 'vue'
            let hookVille= reactive([]);
            const hookPays= reactive([]);
            // Fonction qui traite les erreurs de la requête
            function showError(error) {
                console.log(error)
            }

            function printCountries(){
            fetch("api/countries")
                .then(response => response.json())
                .then((json )=> {
                    let listePays=json._embedded.countries
                    let select = document.getElementById("select")
                    json._embedded.countries.forEach((p)=>{hookPays.push(p)})
                    console.log("HOOK PAYS:", hookPays[0])
                    for(let c=0; c<listePays.length; c++){
                        
                        let listVilles = listePays[c]._links.cities.href
                        
                        select.innerHTML=select.innerHTML + `<option value="${listVilles}"> ${listePays[c].name}</option>`
                    }
                    const url={target:{value:hookPays[0]._links.cities.href}}
                    printVilles(url)
                    })
                .catch(error => showError(error))
                
            //printVilles(hookPays[0]._links.cities.href)
        }
        
       
        
        function deleteVille(ville,index){
            console.log(ville._links.city.href)
            fetch(`${ville._links.city.href}`,{method:'DELETE'})
            .then(()=>{
                hookVille.splice(index,1)
            })
            
        }
        function printVilles(url){
        console.log("URL: ",url)
         fetch(url.target.value)
             .then(response => response.json())
             .then((json )=> {
                let l=hookVille.length
                for(let i=0;i<l;i++){
                    console.log("LONGUEUR", hookVille.length)
                    console.log("i: ",i)
                    hookVille.pop()
                }
                 json._embedded.cities.forEach((v)=>{hookVille.push(v)})
                    console.log(hookVille[0])
                 document.getElementById("tab").style.display="block"
                 return hookVille
                 
                 })
             .catch(error => showError(error))
     }
    
     
        printCountries()
        
</script>
