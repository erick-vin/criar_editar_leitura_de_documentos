<template>
  <div >
   <div class="row">
      <div class="col-md-6">
          <div class="container">
              
                  <div class="form-row">
                    <div class="col mt-5">
                      <input v-model="inputCampo" type="text" class="form-control mb-3" placeholder="Nome do Campo">
                    </div>
                    <div class="col mb-3">
                      <input v-model="inputPrefixo" type="text" class="form-control" placeholder="Prefixo">
                    </div>
                    <div class="col mb-3">
                      <input v-model="inputValor" type="text" class="form-control mb-3" placeholder="Valor">
                    </div>
                    <div class="mb-4 row">
                      <label for="inputPagina" class="col-sm-2 col-form-label">Pagina</label>
                      <div class="col-sm-10">
                        <input type="number" class="form-control" v-model="inputPagina" @value="inputPagina">
                      </div>
                    </div>
                  </div>
                  <div class="col-auto">
                      <button @click="adicionarCampo()" class="btn btn-primary mb-3">Adicionar</button>
                  </div>
                <hr class="mt-4 mb-5">
                <button @click="postGerarJson()" class="btn btn-primary mb-3">Gerar Json</button>
                <table class="table">
                  <thead>
                    <tr>
                      <th scope="col">Campo</th>
                      <th scope="col">Prefixo</th>
                      <th scope="col">Valor</th>
                      <th scope="col">Pagina</th>
                      <th scope="col">Ações</th>
                    </tr>
                  </thead>
                  <tbody class="table-group-divider">
                    
                    <tr v-for="(item, index) in dadosParaEnviar" :key="index">
                      <td>{{item.campo}}</td>
                      <td>{{item.prefixo}}</td>
                      <td>{{item.valor}}</td>
                      <td>{{item.pagina}}</td>
                      <td><button @click="removerCampoTabela(index)" type="button" class="btn btn-outline-primary btn-sm">delete</button></td>
                    </tr>
                  </tbody>
                </table>
          </div>
          
      </div>

      <!-- Segunda Coluna -->
      <div class="col-md-6">
          
          <div class="mb-3">
            
            <!-- Leitura em json começo -->
            <div class="d-flex justify-content-between">
              <label for="exampleFormControlTextarea1" class="form-label">  Json para leitura do Documento</label>
              <div>
                <button @click="adicionarBlocoJson()" class="btn btn-secondary mb-3 justify-content-end mr-5">+ Bloco</button>
                &nbsp;
                <button @click="testarJson()" class="btn btn-success mb-3 justify-content-end mr-5">Testar</button>
              </div>
            </div>
            <div class="form-check form-switch">
              <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckDefault" v-model="modoDesenvolvedor" @click="converterLeituraJson()">
              <label class="form-check-label" for="flexSwitchCheckDefault">Modo desenvolvedor</label>
            </div>
            <div v-if="modoDesenvolvedor">
              <textarea class="form-control" rows="20" v-model="blocosParaLeituraString">
                  
              </textarea>
            </div>
            <!-- Leitura em json Fim -->

            <!-- Leitura com interface começo -->
            <div v-else>
              <div v-for="(item, index) in blocosParaLeituraJson" :key="index" class="mt-4">
                
                <h5>Bloco {{index + 1}}</h5>
                <!-- Detalhes começo -->
                <div class="btn-group" role="group" aria-label="Basic outlined example">
                  <button class="btn btn-outline-primary" type="button" data-bs-toggle="collapse" :data-bs-target="'#collapseDetalhes' + index" aria-expanded="false" aria-controls="collapseExample">
                    Detalhes
                  </button>
                  <button class="btn btn-outline-primary" type="button" data-bs-toggle="collapse" :data-bs-target="'#collapseOptions' + index" aria-expanded="false" aria-controls="collapseExample">
                    Opções
                  </button>
                  <button class="btn btn-outline-primary" type="button" data-bs-toggle="collapse" :data-bs-target="'#collapseFields' + index" aria-expanded="false" aria-controls="collapseExample">
                    Fields
                  </button>
                  <button class="btn btn-outline-primary" type="button" data-bs-toggle="collapse" :data-bs-target="'#collapseCheckList' + index" aria-expanded="false" aria-controls="collapseExample">
                    CheckList
                  </button>
                </div>
                
                <div class="collapse" :id="'collapseDetalhes' + index">
                  <div class="card card-body">
                    <div class="mb-4 row">
                      
                      <!-- Input pagina começo -->
                      <label @for="item.search_pages" class="col-sm-2 col-form-label">Pagina</label>
                      <div class="col-sm-10 mb-2">
                        <input type="number" class="form-control" v-model="item.search_pages" @value="item.search_pages">
                      </div>
                      <!-- Input pagina Fim -->
                      
                      <!-- Input Estilo começo -->
                      <label @for="item.table_style" class="col-sm-2 col-form-label">Estilo</label>
                      <div class="col-sm-10">
                        <select class="form-select" v-model="item.table_style">
                          <option selected value="stream">stream</option>
                          <option value="lattice">lattice</option>
                        </select>
                      </div>
                      <!-- Input Estilo Fim -->

                      <!-- Input pagina começo -->
                     
                      <div class="row mt-2">
                         <label @for="item.search_pages" class="col-sm-2 col-form-label">Area</label>
                        <div class="col-sm-2 mb-2" v-for="(coord, index) in item.table_areas" :key="index">
                          <input type="number" class="form-control" v-model="item.table_areas[index]" @value="item.table_areas[index]">
                        </div>
                      </div>
                      <!-- Input pagina Fim -->

                      <!-- Input Campo invalido começo -->
                      <label @for="item.on_invalid_field" class="col-sm-2 col-form-label">Campo inválido</label>
                      <div class="col-sm-10">
                        <select class="form-select" v-model="item.on_invalid_field">
                          <option selected value="about">about</option>
                          <option value="ignore">ignore</option>
                          <option value="ignore_all">ignore_all</option>
                        </select>
                      </div>
                      <!-- Input Estilo Fim -->

                    </div>
                  </div>
                </div>
                <!-- Detalhes Fim -->

                <!-- Opções Começo -->
                <div class="collapse" :id="'collapseOptions' + index">
                  <div class="card card-body">
                    <div class="mb-4 row">
                      
                      <div class="input-group">
                        <div class="col-sm-8 mb-2">
                          <select class="form-select" v-model="leituraOpcao">
                            <option selected value="fix_upper_margin">fix_upper_margin</option>
                            <option value="fix_lower_margin">fix_lower_margin</option>
                            <option value="line_text">line_text</option>
                            <option value="text">text</option>
                          </select>
                        </div>
                        <div class="col-sm-4 mb-2">
                          <button @click="adicionarOpcaoNaLeitura(index)" class="btn btn-success mb-3">Adicionar</button>
                        </div>
                      </div>

                      <div v-for="(itemOpcao, indexOpcao) in item.options" :key="indexOpcao" class="row mb-2">
                        <label class="col-sm-2 col-form-label">{{indexOpcao}}</label>
                        <div class="col-sm-10">
                          <select class="form-select" v-model="item.options[indexOpcao]">
                            <option selected value="True">Sim</option>
                            <option value="False">Não</option>
                          </select>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <!-- Opções Fim -->
                
                <!-- Fields Começo -->
                <div class="collapse" :id="'collapseFields' + index">
                  <div class="card card-body">
                    <div class="mb-4 row">
                      <h5 class="mb-4">
                          Fields
                        </h5>
                      <div v-for="(field, indexField) in item.fields" :key="indexField">
                        
                        <!-- Input pagina começo -->
                        <div class="row mt-2">
                          <div class="col-sm-3 mb-2" v-for="(dado, indexDado) in field" :key="indexDado">
                            <input type="text" class="form-control" v-model="field[indexDado]" @value="field[indexDado]">
                            
                          </div>
                          <button @click="removerBlocoFields(indexField, index)" class="btn btn-danger mt-3">delete</button>
                          <hr class="mt-3">
                        </div>
                        <!-- Input pagina Fim -->
                        
                      </div>
                      <button @click="adicionarBlocoFields(index)" class="btn btn-secondary mt-3  mr-5">+ Bloco</button>
                    </div>
                  </div>
                </div>
                <!-- Fields Fim -->

                <!-- CheckList Começo -->
                <div class="collapse" :id="'collapseCheckList' + index">
                  <div class="card card-body">
                    <div class="mb-4 row">
                      <h5 class="mb-4">
                          CheckList
                        </h5>
                      <div v-for="(checklist, indexChecklist) in item.checklist" :key="indexChecklist">
                        
                        <!-- Input pagina começo -->
                        <div class="row mt-2">
                          <div class="col-sm-3 mb-2" v-for="(dado, indexDado) in checklist" :key="indexDado">
                            <input type="text" class="form-control" v-model="checklist[indexDado]" @value="checklist[indexDado]">
                            
                          </div>
                          <button @click="removerBlocoChecklist(indexChecklist, index)" class="btn btn-danger mt-3">delete</button>
                          <hr class="mt-3">
                        </div>
                        <!-- Input pagina Fim -->
                        
                      </div>
                      <button @click="adicionarBlocoChecklist(index)" class="btn btn-secondary mt-3  mr-5">+ Bloco</button>
                    </div>
                  </div>
                </div>
                <!-- CheckList Fim -->
              </div>
            </div>
            <!-- Leitura com interface Fim -->
          </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      leituraOpcao : '',
      blocosParaLeituraString : '',
      blocosParaLeituraJson : [
        {
          'search_pages' : '123',
          'table_style': 'stream',
          'table_areas' : [10, 10, 10, 10],
          'on_invalid_field': 'ignore',
          'options' : {
            'fix_upper_margin': 'True',
            'fix_lower_margin': 'True',
            'line_text': 'True'
          },
          'fields' : [
            ['emissao', 'None', '', ''],
            ['emissao2', 'None', '', ''],
          ],
          'checklist' : [
            ['', '']
          ]
        },
        {
          'search_pages' : '456',
          'table_style': 'lattice',
          'table_areas' : [10, 10, 10, 10],
          'on_invalid_field': 'ignore',
          'options' : {
            'fix_upper_margin': 'True',
            'fix_lower_margin': 'True',
            'line_text': 'True'
          },
          'fields' : [],
          'checklist' : [
            
          ]
        },
      ],
      dadosParaEnviar : [],
      dadosParaReceber : '',
      inputCampo: '',
      inputPrefixo: '',
      inputValor: '',
      inputPagina: '1',
      modoDesenvolvedor : false
    }
  },
  props: {
    
  },
  computed: {
    
  },
  methods: {
    converterLeituraJson(){
      if(this.modoDesenvolvedor == true){
        this.blocosParaLeituraJson = JSON.parse(this.blocosParaLeituraString)
        console.log(this.blocosParaLeituraJson)
      }else {
        this.blocosParaLeituraString = JSON.stringify(this.blocosParaLeituraJson, null, 4)
      }
    },
    adicionarCampo(){

      this.dadosParaEnviar.push(
          {
              "campo" : this.inputCampo,
              "prefixo" : this.inputPrefixo,
              "valor" : this.inputValor,
              "pagina" : this.inputPagina
          }
      ),
      this.inputCampo = '',
      this.inputPrefixo = '',
      this.inputValor = '',
      this.inputPagina = '1'
    },

    testarJson(){
      this.$http
      .post('http://127.0.0.1:5000/testarJson', JSON.stringify(this.dadosParaEnviarTeste))
      .then(res => this.restultadoLeituraTeste = res.body)
    },

    removerCampoTabela(index){
      this.dadosParaEnviar.splice(index, 1)
    },

    postGerarJson(){
    this.dadosParaReceber = ''

    this.$http
    .post('http://127.0.0.1:5000/criarleitura', JSON.stringify(this.dadosParaEnviar))
    .then(res => this.dadosParaReceber = res.body)

    console.log(this.dadosParaReceber)
    },

    adicionarBlocoJson(){
      this.blocosParaLeituraJson.push({
        'search_pages': '1',
        'table_style': 'stream',
        'table_areas': [
            0,
            0,
            0,
            0
        ],
        'on_invalid_field': 'ignore',
        'options': {
            'fix_upper_margin': 'True',
            'fix_lower_margin': 'True',
            'line_text': 'True'
        },
        'fields': [
            [
              'campo', '', '', ''
            ]
        ],
        'checklist': []
    })
    },

    adicionarBlocoFields(index){
      this.blocosParaLeituraJson[index].fields.push([
        '', '', '', ''
      ])
    },
    removerBlocoFields(indexField, index){
      this.blocosParaLeituraJson[index].fields.splice(indexField, 1)
    },
    adicionarBlocoChecklist(index){
      this.blocosParaLeituraJson[index].checklist.push([
        '', ''
      ])
    },
    removerBlocoChecklist(indexChecklist, index){
      this.blocosParaLeituraJson[index].checklist.splice(indexChecklist, 1)
    },

    adicionarOpcaoNaLeitura(index){
      this.blocosParaLeituraJson[index].options[this.leituraOpcao] = "True"
    }
  }
}
</script>

<style scoped>

</style>
