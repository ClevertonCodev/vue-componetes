<template>
    <div class="checkboxlist" id="checkBoxListId">
        <div class="selectCheckBoxList" :id="`selectListWidth_${$props.name}`" @click="showCheckbox">
            <select :class="{'disabled-select': $props.disabled}">
                <option>{{listCheck}}</option>
            </select>
            <div class="overSelectCheckBoxList"></div>
        </div>
        <div id="checkBoxesList" :class="`checkBoxesList_${$props.name}`">
            <label class="payt-checkboxlist-label" :key="key" v-for="(item, key) in this.checkboxes">
            <input type="hidden" :name="key" :value="item.value ? 1 : 0">
              <input 
                type="checkbox" 
                :id="key"
                class="payt-checkboxlist"
                :checked="item.value"
                @input="(e) => updateCheckbox(key, e.target.checked)"
                @click="isChecked($event)"
                >{{item.label}}
              <span class="payt-checkmarklist">
                <span class="payt-checkboxlist-checked"></span>
              </span>
            </label>
        </div>
    </div>
</template>
<script>
export default {
    props:[
        'label',
        'values',
        'name',
        'disabled'
    ],
    data(){
        return {
            expanded: false,
            listCheck: '',
            checkboxes: this.$props.values,
            labelText: 'Selecione as opções',
        }
    },
    methods:{
        check(value){
            return value ? true : false;
        },
        isChecked(event){
            let labelSelecionado    = this.$props.values[event.target.id].label;   
            let labelsListSelect    = this.listCheck.split(', ');
            let isExistValue        = labelsListSelect.indexOf(labelSelecionado);

            let textSelecionar = labelsListSelect.indexOf(this.labelText);

            if(textSelecionar >= 0){
                labelsListSelect = [];
            }

            if(event.target.checked){

                if(isExistValue < 0){
                    labelsListSelect.push(labelSelecionado);
                }

                this.listCheck = labelsListSelect.join(", ");
            }else{
                labelsListSelect.splice(isExistValue, 1);
                this.listCheck = labelsListSelect.join(", ");
            }

            if(labelsListSelect.length == 0){
                this.listCheck = this.labelText;
            }
        },
        updateCheckbox(key, newValue) {
            this.$set(this.checkboxes[key], 'value', newValue);
        },
        showCheckbox(){
            if (!this.$props.disabled) {
                let checkboxes      = document.getElementsByClassName(`checkBoxesList_${this.$props.name}`)[0];
                let elementWidth    = document.getElementById(`selectListWidth_${this.$props.name}`).getBoundingClientRect().width;
                
                if(!this.expanded){
                    checkboxes.setAttribute('style', 'visibility: visible; width: '+elementWidth+'px !important;');
                    this.expanded = true;
                }else{
                    checkboxes.style.visibility = "hidden";
                    this.expanded = false;
                }
            }
        },
        showLabelText(itens){
            for(const item in itens){
                if(itens[item].value){
                    return true;
                }
            }

            return false;
        },
        labelSelect(itens){
            let labelOptions = [];
            for(const item in itens){
                if(itens[item].value){
                    labelOptions.push(itens[item].label);
                }
            }

            this.listCheck = labelOptions.join(', ');
        }
    },
    mounted(){
        if(this.showLabelText(this.$props.values)){
            this.labelSelect(this.$props.values);
        }else{
            this.listCheck = this.labelText;
        }
    }
}
</script>
<style scoped>
    .disabled-select {
        background-color: #eee !important;
    }
    .checkboxlist{
        width: 100%;
    }
    .selectCheckBoxList{
        position: relative;
    }
    .selectCheckBoxList select {
        background-color: #FFF;
    }

    .overSelectCheckBoxList {
        position: absolute;
        cursor: pointer;
        left:0; right: 0; top: 0; bottom: 0;
    }

    #checkBoxesList{
        visibility: hidden;
        position: absolute;
        z-index: 9999;
        background-color: #fff;
        border-left: 1px solid #038FE3;
        border-right: 1px solid #038FE3;
        border-bottom: 1px solid #038FE3;
        border-bottom-right-radius: 5px;
        border-bottom-left-radius: 5px;
        width: auto;
    }

    #checkBoxesList label:hover{
        background-color: #038FE3;
        color: #fff !important;
    }

    .payt-checkboxlist-label {
        display: block;
        padding: 6px;
        padding-left: 35px;
        position: relative;
        cursor: pointer;
    }

    .payt-checkboxlist-label input:checked ~ .payt-checkmarklist .payt-checkboxlist-checked{
        background-color: #038FE3;
    }

    .payt-checkboxlist-checked{
        position: absolute;
        height: 17px;
        width: 17px;
        border-radius: 2px;
    }
    .payt-checkboxlist-label input {
        position: absolute;
        opacity: 0;
        cursor: pointer;
        height: 0;
        width: 0;
    }

    .payt-checkmarklist{
        position: absolute;
        top: 4px;
        left: 4px;
        height: 25px;
        width: 25px;
        border-radius: 2px;
        border: 1px solid #038FE3;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #FFF;
    }
</style>