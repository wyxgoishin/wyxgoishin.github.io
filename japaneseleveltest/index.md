# 日本語語彙力診断


<!--more--> 

<div id="question_box" style="margin: 10px 30%; color:black; height:50%; width:50%; background-color:rgb(204, 232, 207);">
    <div style="text-align:center; font-size:large; margin-bottom: 10px; background-color: white">您当前的得分为：<span id="score">100</span></div>
    <div style="text-align:center; margin-bottom:10px; background-color: rgb(41, 36, 33); color:white">
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
    <div style="word-break:break-all; font-size:large;" >
       <div id="question_type" style="background-color: white; margin-bottom:10px;">
           <span>次の語句ともっとも関係の深い文を1～4の中から一つ選びなさい。</span>
        </div>
       <div id="question_text" style="margin-bottom: 10px; background-color:white">
           <ruby><rb>膠</rb><rt>こう</rt></ruby>着
        </div>
        <div id="question_selection1" style="margin-bottom: 10px; background-color:white">
            <input type="radio" name="question_selection"><span>あの二人は仲良しだが、お互いを束縛し合っている。</span>
        </div>
        <div id="question_selection2" style="margin-bottom: 10px; background-color:white">
            <input type="radio" name="question_selection"><span>両者とも自分の主張をゆずらず、交渉が先に進まない。</span>
        </div>
        <div id="question_selection3" style="margin-bottom: 10px; background-color:white">
            <input type="radio" name="question_selection"><span>子どもが机に<ruby><rb>貼</rb><rt>は</rt></ruby>り付けてしまったシールは、なかなかとれない。</span>
        </div>
        <div id="question_selection4" style="margin-bottom: 10px; background-color:white">
            <input type="radio" name="question_selection"><span>新幹線は大雨のため徐行運転をしており、かなり遅れる予定だ。</span>
        </div>
    </div>
    <div style="height:5px"></div>
</div>

{{< script >}}
alert("欢迎游玩「語彙力診断」！本游戏初始得分为 100，一共有 6 个游戏难度等级可以选择，每次切换难度消耗 100 得分。每次答题成功获得 10 得分，反之扣除 10 得分。若得分小于 0，则结束游戏。希望您玩得高兴！")
let question = {qLevel: 1, qType:3, qAns: 2, qIndex: 25};
let level_selects = document.getElementById("question_level");
let score = 100;
let maxScore = 0;
let score_box = document.getElementById("score");

let question_type = document.getElementById("question_type").getElementsByTagName("span")[0];
let question_text = document.getElementById("question_text");
let question_selections = [];
let question_selection_btns = []
for(let i = 1; i <= 4; i++){
	let id = "question_selection" + i;
	let selection = document.getElementById(id);
	question_selections.push(selection.getElementsByTagName("span")[0]);
	question_selection_btns.push(selection.getElementsByTagName("input")[0]);
}

let question_type_texts = [];
let req = new XMLHttpRequest();
req.open("GET", "/ja-xml/question_type_list.xml", false);
req.send(null);
let parser = new DOMParser();
let xmlDoc = parser.parseFromString(req.response, "text/xml");
for(var elem of xmlDoc.getElementsByTagName("string")){
	question_type_texts.push(elem.textContent);
}

let questions = [];
for(let i = 1; i <= 6; i++){
	let req_url = "/ja-xml/question_class_" + i + ".xml";
	req.open("GET", req_url, false);
	req.send(null);
	xmlDoc = parser.parseFromString(req.response, "text/xml");
	questions.push(xmlDoc.getElementsByTagName("array"));
}

let get_nxt_question = function(cur_question, ans_idx, level){
	if(cur_question.qAns != ans_idx){
		alert("很可惜，你答错了本题，扣除 10 点得分！");
		score = score - 10;
		maxScore = Math.max(maxScore, score);
		if(score >= 0){
			score_box.textContent = score;
			return;
		}else{
			alert("得分小于 0，游戏结束！您的最高得分为 " + maxScore + "！");
			level = 5;
			score = 100;
			maxScore = 0;
			level_selects.getElementsByTagName("option")[0].selected = "selected";
			cur_question.qLevel = 1;
		}
	}else{
		if(level != -1){
			if(score >= 100){
				level = (level - 0) + 1;
				score -= 100;
				alert("扣除 100 得分！即将切换到难度等级：" + level + "级！");
				level_selects.getElementsByTagName("option")[level - 1].selected = "selected";
			}else{
				level = cur_question.qLevel - 1;
				alert("每次切换难度等级需要消耗 100 得分，而您当前的得分不足！")
				level_selects.getElementsByTagName("option")[0].selected = "selected";
				return;
			}
		}else{
			alert("恭喜你答对了本题，获得 10 点得分！即将跳转下一题！");
			score += 10;
		}
	}
	score_box.textContent = score;
	let nxt_questions = questions[cur_question.qLevel - 1];
	let nxt_question_idx = cur_question.qIndex;
	let question_nums = nxt_questions.length;
	for(; nxt_question_idx == cur_question.qIndex; ){
		nxt_question_idx = Math.floor(Math.random() * question_nums);
	}
	let nxt_question = nxt_questions[nxt_question_idx];
	
	cur_question.qLevel = nxt_question.getElementsByTagName("ClassNumber")[0].textContent - 1;
	cur_question.qType = nxt_question.getElementsByTagName("TypeNumber")[0].textContent - 0;
	cur_question.qAns = nxt_question.getElementsByTagName("CorrectNumber")[0].textContent - 0;
	cur_question.qIndex = nxt_question_idx;
	question_text.innerHTML = nxt_question.getElementsByTagName("QuestionText")[0].textContent.replaceAll("[", "<").replaceAll("]", ">");
	question_type.textContent = question_type_texts[cur_question.qType];
	
	for(let i = 1; i <= 4; i++){
		let tagName = "QuestionSelection" + i;
		question_selections[i - 1].innerHTML = nxt_question.getElementsByTagName(tagName)[0].textContent.replaceAll("[", "<").replaceAll("]", ">");
	}
}

for(let i = 1; i <= 4; i++){
	question_selection_btns[i - 1].onclick = function(){
		get_nxt_question(question, i, -1);
	};
}

level_selects.onchange = function(){
	get_nxt_question(question, question.qAns, level_selects.options[level_selects.selectedIndex].value)
}
{{< /script >}}

