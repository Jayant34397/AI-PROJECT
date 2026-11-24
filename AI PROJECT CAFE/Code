const $=s=>document.querySelector(s);
const IMG={mac:"images/mac.jpg",pizza:"images/pizza.jpg",garlic:"images/garlic.jpg",tea:"images/tea.jpg",shake:"images/shake.jpg",room:"images/room.jpg"};
const MENU=[
 {n:"Baked Mac & Cheese",p:320,c:"Mains",k:"mac",d:"Elbow pasta, three cheeses, toasted crumb, fresh chives."},
 {n:"Wild Mushroom Pizza",p:420,c:"Mains",k:"pizza",d:"Thin crust, roasted mushrooms, rocket, garlic oil."},
 {n:"Garlic Bread",p:180,c:"Sides",k:"garlic",d:"Thick-cut loaf, herb butter, golden under the grill."},
 {n:"Peach Iced Tea",p:150,c:"Drinks",k:"tea",d:"Black tea, peach, lots of ice, a long straw."},
 {n:"Cold Coffee Shake",p:210,c:"Drinks",k:"shake",d:"Slow-brewed coffee blended thick with vanilla ice cream."}
];
const PANES=[["Mac & Cheese","Baked","mac"],["Mushroom Pizza","Wood-fired","pizza"],["Garlic Bread","Buttery","garlic"],["Peach Iced Tea","Chilled","tea"],["Cold Coffee","Thick shake","shake"],["The room","Window seats","room"]];
const store={get(k,d){try{const v=localStorage.getItem(k);return v?JSON.parse(v):d}catch(e){return d}},set(k,v){try{localStorage.setItem(k,JSON.stringify(v))}catch(e){}}};
function toast(m){const t=$("#toast");t.textContent=m;t.classList.add("on");clearTimeout(toast.t);toast.t=setTimeout(()=>t.classList.remove("on"),2200)}
function el(tag,cls,txt){const e=document.createElement(tag);if(cls)e.className=cls;if(txt!=null)e.textContent=txt;return e}

/* routing */
function route(){
 const h=(location.hash||"#home").slice(1);const id=["home","menu","reviews","visit"].includes(h)?h:"home";
 document.querySelectorAll(".page").forEach(p=>p.classList.toggle("on",p.id===id));
 document.querySelectorAll("nav a").forEach(a=>a.toggleAttribute("aria-current",a.getAttribute("href")==="#"+id));
 document.querySelectorAll("nav a").forEach(a=>{if(a.getAttribute("href")==="#"+id)a.setAttribute("aria-current","page");else a.removeAttribute("aria-current")});
 window.scrollTo(0,0);
}
addEventListener("hashchange",route);

/* lights / theme */
const dark0=matchMedia("(prefers-color-scheme:dark)").matches;
function setLights(on){
 document.documentElement.dataset.theme=on?"dark":"light";
 $("#lamp").setAttribute("aria-pressed",on);$("#lamp").textContent=on?"Lights off":"Lights on";
 $("#bulbs").classList.toggle("lit",on);
}
const bulbs=$("#bulbs");for(let i=0;i<14;i++){const b=el("i","bulb");b.style.transitionDelay=i*40+"ms";bulbs.appendChild(b)}
$("#lamp").onclick=()=>{const on=$("#lamp").getAttribute("aria-pressed")!=="true";setLights(on);toast(on?"Evening mode. Lights on.":"Daylight mode.")};
setLights(dark0);

/* hero panes */
const frame=$("#frame");
PANES.forEach((p,i)=>{const b=el("button","pane");b.style.backgroundImage="url("+IMG[p[2]]+")";b.setAttribute("aria-pressed","false");
 b.append(el("span",null,p[0]),el("small",null,p[1]));b.onclick=()=>pick(i);frame.appendChild(b)});
function pick(i){
 frame.querySelectorAll(".pane").forEach((b,j)=>b.setAttribute("aria-pressed",i===j));
 const m=MENU.find(x=>x.k===PANES[i][2]);const box=$("#pick");
 if(m)box.replaceChildren(el("h3",null,m.n+"  ₹"+m.p),el("p",null,m.d));
 else box.replaceChildren(el("h3",null,"The room"),el("p",null,"Black steel windows, exposed brick and long wooden tables. Ask for a seat by the glass."));
}
pick(0);

/* menu */
let cat="All";const cats=["All",...new Set(MENU.map(m=>m.c))];
function renderTabs(){const t=$("#tabs");t.replaceChildren();cats.forEach(c=>{const b=el("button","tab",c);b.setAttribute("aria-pressed",c===cat);b.onclick=()=>{cat=c;renderTabs();renderItems()};t.appendChild(b)})}
function renderItems(){const w=$("#items");w.replaceChildren();MENU.filter(m=>cat==="All"||m.c===cat).forEach(m=>{
 const d=el("div","item");const im=el("img");im.src=IMG[m.k];im.alt=m.n;
 const h=el("h3");h.append(el("span",null,m.n),el("span",null,"₹"+m.p));d.append(im,h,el("p",null,m.d));w.appendChild(d)})}
renderTabs();renderItems();

/* reviews */
const SEED=[
 {name:"Aanya",dish:"Baked Mac & Cheese",stars:5,text:"Creamy inside, crisp on top. We grabbed the table by the big window and stayed for hours.",t:1731000000000},
 {name:"Rohan",dish:"Garlic Bread",stars:5,text:"Thick, buttery and still hot. Order two, because one disappears fast.",t:1731500000000},
 {name:"Meera",dish:"Cold Coffee Shake",stars:4,text:"Properly thick and not too sweet. Wish the portion was a little bigger.",t:1732000000000},
 {name:"Kabir",dish:"Wild Mushroom Pizza",stars:4,text:"Great crust and lots of mushroom. The room is the best part, all brick and warm light.",t:1732400000000},
 {name:"Ishita",dish:"Peach Iced Tea",stars:3,text:"Refreshing, though it was a bit watery by the end. Lovely seating.",t:1732600000000}
];
let reviews=store.get("fb-reviews",SEED),minStar=0,rate=0;
const sx=n=>"★".repeat(n)+"☆".repeat(5-n);
function renderReviews(){
 const n=reviews.length,avg=n?reviews.reduce((a,r)=>a+r.stars,0)/n:0;
 $("#avg").textContent=avg.toFixed(1);$("#avgStars").textContent=sx(Math.round(avg));$("#count").textContent=n+(n===1?" review":" reviews");
 const d=$("#dist");d.replaceChildren();
 for(let s=5;s>=1;s--){const c=reviews.filter(r=>r.stars===s).length;const b=el("button","bar");b.setAttribute("aria-pressed",minStar===s);
  b.setAttribute("aria-label","Show "+s+" star reviews");const i=el("i");const f=el("b");f.style.width=n?c/n*100+"%":"0";i.appendChild(f);
  b.append(el("span",null,s+" ★"),i,el("span",null,String(c)));b.onclick=()=>{minStar=minStar===s?0:s;renderReviews()};d.appendChild(b)}
 const f=$("#rfilter");f.replaceChildren();
 [["All",0],["5 ★",5],["4 ★",4],["3 or less",-1]].forEach(([l,v])=>{const b=el("button","tab",l);b.setAttribute("aria-pressed",(v===-1?minStar===-1:minStar===v));b.onclick=()=>{minStar=v;renderReviews()};f.appendChild(b)});
 const list=$("#rlist");list.replaceChildren();
 const shown=reviews.filter(r=>minStar===0||(minStar===-1?r.stars<=3:r.stars===minStar)).sort((a,b)=>b.t-a.t);
 if(!shown.length)list.appendChild(el("p","meta","No reviews with that rating yet. Be the first to post one."));
 shown.forEach(r=>{const d=el("div","rev");const top=el("div");const st=el("span","stars",sx(r.stars));st.setAttribute("aria-label",r.stars+" out of 5");
  top.append(st,el("strong",null,"  "+r.name));d.append(top,el("div","meta",r.dish+" · "+new Date(r.t).toLocaleDateString(undefined,{day:"numeric",month:"short",year:"numeric"})),el("p",null,r.text));list.appendChild(d)});
}
const sel=$("#rdish");MENU.forEach(m=>sel.appendChild(new Option(m.n,m.n)));
const ps=$("#pstars");
for(let i=1;i<=5;i++){const b=el("button",null,"★");b.type="button";b.setAttribute("role","radio");b.setAttribute("aria-label",i+" star"+(i>1?"s":""));
 b.onclick=()=>{rate=i;[...ps.children].forEach((x,j)=>x.classList.toggle("on",j<i))};
 b.onmouseenter=()=>[...ps.children].forEach((x,j)=>x.style.color=j<i?"var(--amber)":"");
 b.onmouseleave=()=>[...ps.children].forEach(x=>x.style.color="");ps.appendChild(b)}
$("#rform").onsubmit=e=>{e.preventDefault();
 const name=$("#rname").value.trim(),text=$("#rtext").value.trim();
 if(!rate){$("#err").textContent="Choose a star rating first.";return}
 if(!name){$("#err").textContent="Add your name so others know who wrote it.";return}
 if(text.length<10){$("#err").textContent="Write at least a short sentence about your visit.";return}
 $("#err").textContent="";reviews.push({name,dish:sel.value,stars:rate,text,t:Date.now()});store.set("fb-reviews",reviews);
 e.target.reset();rate=0;[...ps.children].forEach(x=>x.classList.remove("on"));minStar=0;renderReviews();toast("Review posted. Thank you!")};
renderReviews();

/* visit */
const H=[["Monday",8,22],["Tuesday",8,22],["Wednesday",8,22],["Thursday",8,22],["Friday",8,23],["Saturday",8,23],["Sunday",9,23]];
const today=(new Date().getDay()+6)%7,tb=$("#hours");
const fmt=h=>(h%12||12)+(h<12?" am":" pm");
H.forEach((r,i)=>{const tr=el("tr",i===today?"today":"");tr.append(el("td",null,r[0]),el("td",null,fmt(r[1])+" to "+fmt(r[2])));tb.appendChild(tr)});
const hr=new Date().getHours(),o=H[today];
$("#openNow").textContent=(hr>=o[1]&&hr<o[2])?"Open now. Closes at "+fmt(o[2]):"Closed now. Opens at "+fmt(o[1])+" today";
$("#openNow").disabled=true;
route();
