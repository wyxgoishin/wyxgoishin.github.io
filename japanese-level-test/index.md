# 日本語語彙力診断


<!--more--> 
<div class="question_warpper">
	<div class="question_box" id="question_box">
		<div class="header">您当前的得分为：<span id="score">0</span></div>
		<div class="headerSelect">
			<span>请选择题目难度：</span>
			<select id="question_level">
				<option value=0>１級</option>
				<option value=1>２級</option>
				<option value=2>３級</option>
				<option value=3>４級</option>
				<option value=4>５級</option>
				<option value=5>６級</option>
			</select>
		</div>
		<div class="main">
			<div id="question_type" class="question_type">
				<span>次の語句ともっとも関係の深い文を1～4の中から一つ選びなさい。</span>
			</div>
			<div id="question_text" class="question_text">
				<ruby>
					<rb>膠</rb>
					<rt>こう</rt>
				</ruby>着
			</div>
			<div id="question_selection1" class="question_selection">
				<span class="question_selection_span">あの二人は仲良しだが、お互いを束縛し合っている。</span>
			</div>
			<div id="question_selection2" class="question_selection">
				<span class="question_selection_span">両者とも自分の主張をゆずらず、交渉が先に進まない。</span>
			</div>
			<div id="question_selection3" class="question_selection">
				<span class="question_selection_span">子どもが机に<ruby>
						<rb>貼</rb>
						<rt>は</rt>
					</ruby>り付けてしまったシールは、なかなかとれない。</span>
			</div>
			<div id="question_selection4" class="question_selection">
				<span class="question_selection_span">新幹線は大雨のため徐行運転をしており、かなり遅れる予定だ。</span>
			</div>
			<div id="question_selection_bottom" style="height:5px"></div>
		</div>
	</div>
</div>
{{< script >}}
let jlt = {};

jlt.refresh_rbt = function(){
	let rbs = document.getElementsByTagName("rb");
	let rts = document.getElementsByTagName("rt");
	let rubys = document.getElementsByTagName("ruby");
	for(let rb of rbs){
		if(rb.parentElement.parentElement.tagName != "DIV"){
			rb.style.backgroundColor = "rgba(225, 195, 158)";
		}else{
			rb.style.backgroundColor = "rgb(184,208,216)";
		}
		rb.style.color = "black";
	}
	for(let rt of rts){
		rt.style.color = "black";
	}
	for(let ruby of rubys){
		let ruby_class = ruby.parentElement.className;
		if(ruby_class == "question_selection_span"){
			ruby.style.backgroundColor = "rgb(225, 195, 158)";
		}else if(ruby_class == "question_text"){
			ruby.style.backgroundColor = "rgb(184,208,216)";
		}
	}
}
jlt.refresh_rbt();

jlt.cssId = 'jlt';
if (!document.getElementById(jlt.cssId))
{
    let head  = document.getElementsByTagName('head')[0];
    let link  = document.createElement('link');
    link.id   = jlt.cssId;
    link.rel  = 'stylesheet';
    link.type = 'text/css';
    link.href = '/css/jlt.css';
    link.media = 'all';
    head.appendChild(link);
}

alert("欢迎游玩「語彙力診断」！本游戏初始得分为 100，一共有 6 个游戏难度等级可以选择，每次切换难度消耗 100 得分。每次答题成功获得 10 得分，反之扣除 10 得分。若得分小于 0，则结束游戏。希望您玩得高兴！")
jlt.question = {qLevel: 0, qType:3, qAns: 2, qIndex: 25};
jlt.level_selects = document.getElementById("question_level");
jlt.score = 100;
jlt.maxScore = 0;
jlt.score_box = document.getElementById("score");
jlt.score_box.textContent = jlt.score;

jlt.question_type = document.getElementById("question_type").getElementsByTagName("span")[0];
jlt.question_text = document.getElementById("question_text");
jlt.question_selections = [];
for(let i = 1; i <= 4; i++){
	let id = "question_selection" + i;
	let selection = document.getElementById(id);
	jlt.question_selections.push(selection.getElementsByTagName("span")[0]);
}

jlt.question_type_texts = [];
jlt.req = new XMLHttpRequest();
jlt.req.open("GET", "/jlt/question_type_list.xml", false);
jlt.req.send(null);
jlt.parser = new DOMParser();
jlt.xmlDoc = jlt.parser.parseFromString(jlt.req.response, "text/xml");
for(let elem of jlt.xmlDoc.getElementsByTagName("string")){
	jlt.question_type_texts.push(elem.textContent.replaceAll("[", "<").replaceAll("]", ">"));
}

jlt.questions = [];
for(let i = 1; i <= 6; i++){
	let req_url = "/jlt/question_class_" + i + ".xml";
	jlt.req.open("GET", req_url, false);
	jlt.req.send(null);
	jlt.xmlDoc = jlt.parser.parseFromString(jlt.req.response, "text/xml");
	jlt.questions.push(jlt.xmlDoc.getElementsByTagName("array"));
}

jlt.get_nxt_question = function(ans_idx, level){
	if(jlt.question.qAns != ans_idx){
		alert("很可惜，你答错了本题，扣除 10 点得分！");
		jlt.score = jlt.score - 10;
		if(jlt.score >= 0){
			jlt.score_box.textContent = jlt.score;
			return;
		}else{
			alert("得分小于 0，游戏结束！您的最高得分为 " + jlt.maxScore + "（注：初始得分为 100）！");
			level = 5;
			jlt.score = 100;
			jlt.maxScore = 0;
			jlt.level_selects.getElementsByTagName("option")[0].selected = "selected";
			jlt.question.qLevel = 1;
		}
	}else{
		if(level != -1){
			if(jlt.score >= 100){
				jlt.score -= 100;
				let t = (level - 0) + 1;
				alert("扣除 100 得分！即将切换到难度等级：" + t + "级！");
				jlt.level_selects.getElementsByTagName("option")[level].selected = "selected";
				jlt.question.qLevel = level;
			}else{
				level = jlt.question.qLevel;
				alert("每次切换难度等级需要消耗 100 得分，而您当前的得分不足！")
				jlt.level_selects.getElementsByTagName("option")[level].selected = "selected";
				return;
			}
		}else{
			alert("恭喜你答对了本题，获得 10 点得分！即将跳转下一题！");
			jlt.score += 10;
			jlt.maxScore = Math.max(jlt.score, jlt.maxScore);
		}
	}
	jlt.score_box.textContent = jlt.score;
	let nxt_questions = jlt.questions[jlt.question.qLevel];
	let nxt_question_idx = jlt.question.qIndex;
	let question_nums = nxt_questions.length;
	for(; nxt_question_idx == jlt.question.qIndex; ){
		nxt_question_idx = Math.floor(Math.random() * question_nums);
	}
	let nxt_question = nxt_questions[nxt_question_idx];
	
	jlt.question.qLevel = nxt_question.getElementsByTagName("ClassNumber")[0].textContent - 2;
	jlt.question.qType = nxt_question.getElementsByTagName("TypeNumber")[0].textContent - 0;
	jlt.question.qAns = nxt_question.getElementsByTagName("CorrectNumber")[0].textContent - 0;
	jlt.question.qIndex = nxt_question_idx;
	jlt.question_text.innerHTML = nxt_question.getElementsByTagName("QuestionText")[0].textContent.replaceAll("[", "<").replaceAll("]", ">");
	jlt.question_type.innerHTML = jlt.question_type_texts[jlt.question.qType];
	
	for(let i = 1; i <= 4; i++){
		let tagName = "QuestionSelection" + i;
		jlt.question_selections[i - 1].innerHTML = nxt_question.getElementsByTagName(tagName)[0].textContent.replaceAll("[", "<").replaceAll("]", ">");
	}
	
	jlt.refresh_rbt();
}

for(let i = 1; i <= 4; i++){
	let id = "question_selection" + i;
	let selection = document.getElementById(id);
	selection.onclick = function(){
		jlt.get_nxt_question(i, -1);
	};
}

jlt.level_selects.onchange = function(){
	jlt.get_nxt_question(jlt.question.qAns, jlt.level_selects.options[jlt.level_selects.selectedIndex].value)
}
{{< /script >}}

