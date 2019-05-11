<template>
    <v-menu offset-y :close-on-content-click="false" class="mx-1">
                    <v-badge overlap slot="activator" color="deep-orange" class="v-badge-sem-topright">
                        <span slot="badge" v-if="itens.length">{{itens.length}}</span>

                        <v-btn icon><v-icon>notifications</v-icon></v-btn>
                    </v-badge>
                        
                    <v-list two-line>
                        
                        <template v-for="(item, index) in itens">
                            
                            <v-alert :key="index" :value="true" :color="item.type || 'info'" outline class="alerta-mensagens ma-0 mb-2 pa-0">
                                <v-list-tile avatar ripple @click="()=>({})" class="v-list-tile">
                                    
                                    <v-list-tile-avatar v-if="item.avatar.icon">
                                        <v-icon>{{ item.avatar.icon }}</v-icon>
                                    </v-list-tile-avatar>

                                    <v-list-tile-content>
                                        <v-list-tile-sub-title v-if="item.content.sent_at"> {{ item.content.sent_at }} </v-list-tile-sub-title>

                                        <v-list-tile-title>{{ item.content.title || '' }}</v-list-tile-title>

                                        <v-list-tile-sub-title v-if="item.content.message">{{ item.content.message | truncate(75) }}</v-list-tile-sub-title>

                                    </v-list-tile-content>

                                    <v-list-tile-action>
                                        <!-- marca mensagens lidas -->
                                        <v-tooltip bottom :disabled="item.action.read?false:true">
                                            <v-btn slot="activator" dark flat icon :color="item.action.read ? 'green' : 'grey'" >
                                                <fa5-icon icon="circle" size="xs" />
                                            </v-btn>
                                            <span>Lida em: {{item.action.read_at}}</span>
                                        </v-tooltip>
                                    </v-list-tile-action>

                                </v-list-tile>
                            </v-alert>

                        </template>
                        <div v-if=" ! itens.length">
                            <v-list-tile avatar ripple @click="()=>({})" class="v-list-tile">
                                <v-list-tile-avatar> <v-icon>close</v-icon> </v-list-tile-avatar>

                                <v-list-tile-content>
                                    <v-list-tile-title>Não há mensagens.</v-list-tile-title>
                                </v-list-tile-content>
                            </v-list-tile>
                        </div>
                    </v-list>
                </v-menu>
</template>

<script>
    export default {
        props:{
            mensagens: String,
        },
        data: function(){
            return {
                /** Modelo
                    {
                        type:'error',
                        avatar: {
                            icon:null,
                        },
                        content: {
                            title: 'Ali Connors',
                            message: "I'll be in your neighborhood doing errands this weekend. Do you want to hang out?",
                            sent_at: '15 min',
                        },
                        action: {
                            read:true,
                            read_at:'15 min',
                        },
                    },
                */
               itens: this.formataItens(this.mensagens)
            }
        },
        methods: {
            formataItens(mensagens){
                if(typeof mensagens=='object'){
                    return mensagens
                }
                else if(! mensagens.trim() || mensagens=='null'){
                    return []
                }
                else if(typeof mensagens === 'string') {                    
                    return JSON.parse(mensagens)
                }
            }
        },
        mounted() {
            
        }
    }
</script>
