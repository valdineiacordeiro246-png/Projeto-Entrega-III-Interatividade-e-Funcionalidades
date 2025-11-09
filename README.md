# Projeto-Entrega-III-Interatividade-e-Funcionalidades
Este projeto foi desenvolvido com parte da diciplina de Desenvolvimento web 

## Fucionalidades
cadastro de usuarios 
envio de formulares intregação com banco de dados 

 ## Tecnologia utilizada 
HTML
CSS
JavScript

## Autora 
Valdineia Cordeiro Reinaldo 

 entregaIII/
 ├─ index.html
 ├─ README.md
 ├─ css/
 │  └─ style.css
 ├─ js/
 │  ├─ main.js           # inicialização (module)
 │  ├─ router.js         # roteador hash-based
 │  ├─ templates.js      # sistema de templates e templates
 │  ├─ validation.js     # validação e mensagens
 │  └─ storage.js        # funções utilitárias para localStorage
 ├─ images/
 │  └─ logo.png (opcional)
 └─ assets/
   └─ (opcional)
 Arquivos (copie/cole nos arquivos correspondentes)
 index.html
 <!doctype html>
 <html lang="pt-BR">
 <head>
 <meta charset="utf-8" />
 <meta name="viewport" content="width=device-width,initial-scale=1" />
 <title>Entrega III — Interatividade</title>
 <link rel="stylesheet" href="css/style.css">
 </head>
 <body>
 <header class="site-header">
 <img src="images/logo.png" alt="logo" class="logo"
 onerror="this.style.display='none'">
 <nav>
 <a href="#/" data-link>Home</a>
 1
<a href="#/form" data-link>Formulário</a>
 <a href="#/about" data-link>Sobre</a>
 </nav>
 </header>
 <main id="app" class="container">
 <!-- Aqui o SPA renderiza as views -->
 </main>
 <footer class="site-footer">Entrega III — Interatividade e 
Funcionalidades</footer>
 <script type="module" src="js/main.js"></script>
 </body>
 </html>
 css/style.css
 :root{--bg:#f7f7fb;--card:#fff;--accent:#2255a4;--muted:#666}
 *{box-sizing:border-box}
 body{font-family:Inter, system-ui, Arial, sans-serif;background:var(-
bg);margin:0;color:#111}
 .site-header{display:flex;align-items:center;gap:1rem;padding:
 1rem;background:var(--card);box-shadow:0 1px 4px rgba(0,0,0,.06)}
 .logo{height:40px}
 nav a{margin-right:1rem;text-decoration:none;color:var(--accent)}
 .container{max-width:900px;margin:2rem auto;padding:1rem}
 .card{background:var(--card);padding:1.25rem;border-radius:8px;box-shadow:0
 6px 18px rgba(0,0,0,.06)}
 .form-row{display:flex;flex-direction:column;margin-bottom:.75rem}
 label{font-weight:600;margin-bottom:.25rem}
 input,select,textarea{padding:.5rem;border:1px solid #dcdce6;border-radius:
 6px}
 .error{color:#b00020;font-size:.9rem;margin-top:.25rem}
 .success{color:green}
 .btn{display:inline-block;padding:.5rem 1rem;border-radius:
 8px;border:none;background:var(--accent);color:#fff;text-decoration:none}
 .small-muted{font-size:.9rem;color:var(--muted)}
 .field-with-error{border-color:#b00020}
 .field-help{font-size:.85rem;color:var(--muted)}
 /* responsivo */
 @media (max-width:600px){.site-header{flex-direction:column;align-items:flex
start}}
 2
js/templates.js
 // Sistema simples de templates (ES Modules)
 export const templates = {
 home: ({savedCount=0} = {}) => `
    <section class="card">
      <h1>Bem-vinda à Entrega III</h1>
      <p class="small-muted">SPA simples com sistema de templates, roteamento 
hash e validação de formulários.</p>
      <p>Formulários salvos: <strong>${savedCount}</strong></p>
      <p><a class="btn" href="#/form">Ir para o formulário</a></p>
    </section>
  `,
 form: (data = {}) => `
    <section class="card">
      <h2>Formulário — Verificação de consistência</h2>
      <form id="main-form" novalidate>
        <div class="form-row">
          <label for="nome">Nome completo</label>
          <input id="nome" name="nome" type="text" value="${data.nome ||
 ''}" required />
          <div class="error" data-error-for="nome"></div>
        </div>
        <div class="form-row">
          <label for="email">E‑mail</label>
          <input id="email" name="email" type="email" value="${data.email ||
 ''}" required />
          <div class="error" data-error-for="email"></div>
        </div>
        <div class="form-row">
          <label for="emailConfirm">Confirme o e‑mail</label>
          <input id="emailConfirm" name="emailConfirm" type="email" value="$
 {data.emailConfirm || ''}" required />
          <div class="error" data-error-for="emailConfirm"></div>
        </div>
        <div class="form-row">
          <label for="idade">Idade</label>
          <input id="idade" name="idade" type="number" min="1" max="120" 
value="${data.idade || ''}" />
          <div class="error" data-error-for="idade"></div>
        </div>
        <div class="form-row">
          <label for="genero">Gênero</label>
          <select id="genero" name="genero">
            <option value="">— selecione —</option>
 3
            <option value="f">Feminino</option>
            <option value="m">Masculino</option>
            <option value="o">Outro</option>
          </select>
          <div class="error" data-error-for="genero"></div>
        </div>
        <div class="form-row">
          <button class="btn" type="submit">Salvar</button>
          <button id="clear-storage" type="button" class="btn" 
style="background:#666;margin-left:.5rem">Limpar salvos</button>
        </div>
        <div id="form-message" class="field-help"></div>
      </form>
    </section>
  `,
 about: () => `
    <section class="card">
      <h2>Sobre</h2>
      <p>Projeto demonstrativo para a Entrega III. Implementa: DOM, 
templates, SPA, validação e armazenamento local.</p>
      <p class="small-muted">Faça o fork e suba para um repositório público 
no GitHub.</p>
    </section>
  `
 };
 export function renderTemplate(name, data){
 const tpl = templates[name];
 if(!tpl) return `<p>Template "${name}" não encontrado.</p>`;
 return tpl(data);
 }
 js/router.js
 import { renderTemplate } from './templates.js';
 import { loadAll } from './storage.js';
 const appEl = document.getElementById('app') ||
 document.querySelector('#app');
 export function goTo(route){
 location.hash = '#/' + route.replace(/^\/?/, '')
 }
 function parseHash(){
 const hash = location.hash.replace(/^#\/?/, '');
 4
return hash || '';
 }
 async function mount(route){
 // carregar dados do storage para exibir contadores etc.
 const all = loadAll();
 const savedCount = all ? all.length : 0;
 if(route === '' || route === undefined){
 appEl.innerHTML = renderTemplate('home', {savedCount});
 } else if(route === 'form'){
 // para o formulário passamos dados vazios por enquanto
 appEl.innerHTML = renderTemplate('form', {});
 // depois emitimos evento para quem quiser ligar a lógica do form
 window.dispatchEvent(new CustomEvent('spa:view-mounted',{detail:
 {view:'form'}}));
 } else if(route === 'about'){
 appEl.innerHTML = renderTemplate('about', {});
 } else {
 appEl.innerHTML = `<section class="card"><h2>Página não encontrada</h2></
 section>`;
 }
 }
 export function startRouter(){
 function onHash(){
 const r = parseHash();
 mount(r);
 }
 window.addEventListener('hashchange', onHash);
 // inicializar
 onHash();
 }
 js/validation.js
 import { saveEntry, loadAll, clearAll } from './storage.js';
 function setError(input, message){
 const err = document.querySelector(`[data-error-for="${input.name}"]`);
 if(err){ err.textContent = message; }
 input.classList.add('field-with-error');
 }
 function clearErrors(form){
 form.querySelectorAll('[data-error-for]').forEach(e=>e.textContent='');
 form.querySelectorAll('.field-with
error').forEach(i=>i.classList.remove('field-with-error'));
 5
}
 export function attachFormLogic(){
 const form = document.getElementById('main-form');
 if(!form) return;
 const messageEl = document.getElementById('form-message');
 const clearBtn = document.getElementById('clear-storage');
 form.addEventListener('submit', (ev)=>{
 ev.preventDefault();
 clearErrors(form);
 messageEl.textContent = '';
 const fd = new FormData(form);
 const data = Object.fromEntries(fd.entries());
 let valid = true;
 // validações básicas
 if(!data.nome || data.nome.trim().length < 3){
 setError(form.nome, 'Digite pelo menos 3 caracteres para o nome.');
 valid = false;
 }
 if(!data.email || !/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(data.email)){
 setError(form.email, 'E‑mail inválido.');
 valid = false;
 }
 if(!data.emailConfirm || data.emailConfirm !== data.email){
 setError(form.emailConfirm, 'Os e‑mails não conferem.');
 valid = false;
 }
 if(data.idade){
 const n = Number(data.idade);
 if(Number.isNaN(n) || n < 1 || n > 120){
 setError(form.idade, 'Idade precisa ser entre 1 e 120.');
 valid = false;
 }
 }
 if(!data.genero){
 setError(form.genero, 'Escolha um gênero.');
 valid = false;
 }
 // verificação de consistência adicional de exemplo:
 // por exemplo: se idade < 18, obrigar confirmação extra (exemplo 
simples)
 6
if(data.idade && Number(data.idade) < 18){
 // demonstra como forçar consistência adicional
 // aqui só mostramos mensagem mas poderia habilitar campos extras
 messageEl.textContent = 'Observação: usuário menor de idade — 
certifique-se de ter autorização.';
 }
 if(!valid){
 messageEl.className = 'field-help';
 return;
 }
 // salvar
 saveEntry(data);
 messageEl.className = 'success';
 messageEl.textContent = 'Dados válidos! Salvo no armazenamento local.';
 // atualizar contador no Home quando voltar
 window.dispatchEvent(new CustomEvent('spa:data-changed'));
 });
 clearBtn.addEventListener('click', ()=>{
 if(confirm('Deseja limpar todos os registros salvos?')){
 clearAll();
 window.dispatchEvent(new CustomEvent('spa:data-changed'));
 alert('Registros limpos.');
 }
 });
 }
 js/storage.js
 const KEY = 'entrega3_entries_v1';
 export function loadAll(){
 try{const raw = localStorage.getItem(KEY); return raw?JSON.parse(raw):[];}
 catch(e){return []}
 }
 export function saveAll(arr){ localStorage.setItem(KEY,
 JSON.stringify(arr)); }
 export function saveEntry(obj){
 const arr = loadAll();
 arr.push({id:Date.now(), ...obj});
 saveAll(arr);
 }
 7
export function clearAll(){ localStorage.removeItem(KEY); }
 js/main.js
 import { startRouter } from './router.js';
 import { attachFormLogic } from './validation.js';
 import { loadAll } from './storage.js';
 // Inicia o roteador e conecta listeners para eventos do SPA
 startRouter();
 // Quando uma view for montada, ligamos a lógica necessária
 window.addEventListener('spa:view-mounted', (e)=>{
 if(e.detail.view === 'form'){
 attachFormLogic();
 }
 });
 // quando dados mudarem, força re-render no home (simples)
 window.addEventListener('spa:data-changed', ()=>{
 // se estivermos na home, re-renderizar
 if(location.hash === '' || location.hash === '#/' || location.hash === '#')
 {
 // força atualização
 location.reload();
 }
 });
 // Garantir que se o usuário abrir diretamente #/form o form seja 
inicializado
 // (o router já dispara evento de view-mounted quando renderiza)
 README.md (sugestão)
 # Entrega III — Interatividade e Funcionalidades
 Projeto para a Entrega III: SPA simples em JavaScript.
 ## Como rodar localmente
 1. Abra um terminal na pasta do projeto.
 2. Inicie um servidor local (recomendado):- Com Python 3: `python -m http.server 5500`- Ou use a extensão Live Server do VSCode.
 8
3. Acesse `http://localhost:5500`.
 ## Como subir para o GitHub
 ```bash
 git init
 git add .
 git commit -m "Entrega III - primeira versão"
 # criar repositório público no GitHub via web e copiar a URL, por exemplo:
 # git remote add origin https://github.com/valdineiacordeiro246-png/entregaIII.git
 git branch -M main
 git push -u origin main
