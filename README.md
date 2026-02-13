<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>영어회화 스피드퀴즈 Level 2</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            -webkit-user-select: none; -moz-user-select: none; -ms-user-select: none; user-select: none;
            -webkit-touch-callout: none; -webkit-tap-highlight-color: transparent;
            word-break: keep-all; background-color: #f8fafc; font-family: system-ui, -apple-system, sans-serif;
        }
        .card { perspective: 1000px; }
        .card-inner {
            position: relative; width: 100%; height: 320px;
            transition: transform 0.6s; transform-style: preserve-3d; cursor: pointer;
        }
        .card.flipped .card-inner { transform: rotateY(180deg); }
        .card-front, .card-back {
            position: absolute; width: 100%; height: 100%;
            backface-visibility: hidden; display: flex; flex-direction: column;
            align-items: center; justify-content: center; border-radius: 1.5rem;
            padding: 2rem; box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
        }
        .card-back { transform: rotateY(180deg); }
        .active-tab { background-color: #4f46e5 !important; color: white !important; }
        .star-checked { color: #fbbf24 !important; fill: #fbbf24 !important; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
    </style>
</head>
<body oncontextmenu="return false">

    <div class="max-w-md mx-auto px-4 py-6">
        <header class="text-center mb-6">
            <h1 class="text-2xl font-black text-indigo-600 tracking-tight italic">영어회화 스피드퀴즈</h1>
            <p class="text-[10px] text-slate-400 mt-1 font-bold">DAY 076 - 100</p>
        </header>

        <div class="flex space-x-2 mb-6 overflow-x-auto pb-2 no-scrollbar font-bold text-xs">
            <button onclick="selectWeek('w16')" id="tab-w16" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100 active-tab">Day 76-80</button>
            <button onclick="selectWeek('w17')" id="tab-w17" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 81-85</button>
            <button onclick="selectWeek('w18')" id="tab-w18" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 86-90</button>
            <button onclick="selectWeek('w19')" id="tab-w19" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 91-95</button>
            <button onclick="selectWeek('w20')" id="tab-w20" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 96-100</button>
            <button onclick="selectWeek('total')" id="tab-total" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">전체 범위</button>
        </div>

        <div id="setup-screen" class="bg-white rounded-3xl p-8 shadow-xl border border-slate-100 text-center">
            <div id="selected-week-title" class="text-indigo-500 font-black text-xl mb-6 italic tracking-widest uppercase">DAY 76-80</div>
            <div class="space-y-4">
                <button onclick="startQuiz('mild')" class="w-full py-5 bg-emerald-500 text-white rounded-2xl font-black shadow-lg shadow-emerald-100 active:scale-95 transition">순한맛 시작</button>
                <button onclick="startQuiz('spicy')" class="w-full py-5 bg-orange-500 text-white rounded-2xl font-black shadow-lg shadow-orange-100 active:scale-95 transition">매운맛 시작</button>
            </div>
            <div class="mt-8 p-4 bg-slate-50 rounded-xl text-[11px] text-slate-400 text-left leading-relaxed">
                <p class="mb-2 text-indigo-600 font-bold underline">학습 모드 설명</p>
                <p>• <strong>순한맛</strong>: 대표예제 + Model examples (교재1)</p>
                <p>• <strong>매운맛</strong>: 위 구성 + Small talk, Cases in point, Further studies 전체 포함</p>
                <p class="mt-2 text-rose-400 font-bold italic">※ 선택 구간에서 랜덤 10문제가 출제됩니다.</p>
            </div>
        </div>

        <div id="quiz-screen" class="hidden">
            <div class="flex justify-between items-center mb-6 px-2">
                <span id="progress-text" class="px-3 py-1 bg-indigo-100 text-indigo-600 rounded-full text-xs font-bold">1 / 10</span>
                <button onclick="location.reload()" class="text-slate-400 font-bold text-xs uppercase">Exit ✕</button>
            </div>

            <div id="card-container" class="card" onclick="flipCard()">
                <div class="card-inner">
                    <div class="card-front bg-white border border-slate-100">
                        <span class="text-indigo-400 font-black text-[10px] tracking-widest mb-6 uppercase">Korean</span>
                        <p id="q-korean" class="text-lg font-bold leading-snug px-4 text-center"></p>
                        <p class="mt-10 text-slate-300 text-xs font-bold animate-pulse">터치하여 정답 확인</p>
                    </div>
                    <div class="card-back bg-indigo-600 text-white">
                        <button id="star-btn" onclick="toggleStar(event)" class="absolute top-6 right-6 text-white/40 transition-all">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-9 w-9" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                                <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14l-5-4.87 6.91-1.01L12 2z"/>
                            </svg>
                        </button>
                        <span class="text-indigo-200 font-black text-[10px] tracking-widest mb-6 uppercase">English</span>
                        <p id="q-english" class="text-lg font-black leading-snug px-4 mb-8 text-center"></p>
                        <button onclick="event.stopPropagation(); playTTS();" class="w-16 h-16 bg-white/20 rounded-full flex items-center justify-center active:scale-90 transition shadow-inner">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                            </svg>
                        </button>
                        <div id="q-info" class="absolute bottom-6 text-[9px] text-indigo-300 font-bold uppercase tracking-widest italic"></div>
                    </div>
                </div>
            </div>
            <button id="next-btn" onclick="nextQuestion()" class="w-full mt-8 py-5 bg-indigo-600 text-white rounded-2xl font-black shadow-xl hidden active:scale-95 transition">다음 문제 NEXT</button>
        </div>

        <div id="result-screen" class="hidden bg-white rounded-3xl p-6 shadow-xl border border-slate-100">
            <h2 class="text-center font-black text-xl text-slate-800 mb-8 tracking-tighter">학습 완료 리포트</h2>
            <div id="starred-list" class="space-y-4 max-h-[400px] overflow-y-auto pr-2 custom-scroll"></div>
            <button onclick="location.reload()" class="w-full mt-10 py-5 bg-slate-900 text-white rounded-2xl font-bold active:scale-95 transition">메인으로</button>
        </div>
    </div>

    <script>
        const allData = [
            // Day 076
            {d:76, s:"교재1", k:"(지갑을 잃어 돈을 빌렸을 때) 어젯밤 일은 미안해. 내가 지갑 찾으면 바로 갚을게.", e:"Sorry about last night. I’ll definitely pay you back, as soon as I find my wallet."},
            {d:76, s:"교재1", k:"(접촉 사고를 낸 운전자가) 어제 일은 미안합니다. 걱정 마세요. 제 보험회사에서 모든 비용을 지불해 줄 겁니다.", e:"Sorry about yesterday. Please don’t worry. I’m sure my insurance will cover everything."},
            {d:76, s:"교재1", k:"(실수로 상대방에게 커피를 쏟았을 때) 오늘 아침 일은 미안해. 앞으로는 (커피) 좀 조심해야겠어.", e:"Sorry about this morning. I should be more careful with my coffee."},
            {d:76, s:"교재1", k:"(약속을 못 지켰을 때) 오늘 아침엔 죄송했어요. 알람 맞춰 놓는 걸 깜박했어요.", e:"Sorry about this morning. I forgot to set my alarm."},
            {d:76, s:"교재1", k:"(아내 생일을 깜박한 남편이) 어제는 미안했어. 이번 주말에 뭐 사러 가자.", e:"Sorry about yesterday. I’ll take you shopping this weekend."},
            {d:76, s:"교재4", k:"왜 이렇게 늦었어?", e:"What kept you so long?"},
            {d:76, s:"교재4", k:"길이 너무 막혔어.", e:"Traffic was terrible."},
            {d:76, s:"교재4", k:"길이 너무 막혔어.", e:"Traffic was so bad."},
            {d:76, s:"교재4", k:"서울은 물가가 비싸.", e:"Seoul is expensive."},
            {d:76, s:"교재4", k:"백화점 물건은 너무 비싸.", e:"Department stores are super expensive."},
            {d:76, s:"대표", k:"아까는 미안. 휴대폰 배터리가 나가서.", e:"Sorry about earlier. My phone died."},

            // Day 077
            {d:77, s:"교재1", k:"아직 좀 더 가야 해. 10분 늦어.", e:"I am not there yet. I am running 10 minutes late."},
            {d:77, s:"교재1", k:"휴게소에 화장실이 있을 거야. 근데 아직 더 가야 해.", e:"The rest area will have a bathroom, but we’re not quite there yet."},
            {d:77, s:"교재1", k:"마스크 의무화를 해제하려면 일일 확진자 수가 오천 명 아래로 떨어져야 합니다. 아직은 그 정도가 아닙니다.", e:"Before lifting the mask mandate, cases should be at less than 5,000 a day. We are not there yet."},
            {d:77, s:"교재1", k:"그 여자 좋긴 한데, 아직 결혼할 정돈 아니야. 한 1년쯤 후에나 결혼할 듯.", e:"I like her, but we’re not there yet. Maybe we can get married in a year or so."},
            {d:77, s:"교재1", k:"기술이 많이 발전하긴 했지만, 아직은 부족합니다.", e:"The technology is advanced enough, but we are not there yet."},
            {d:77, s:"교재4", k:"서울의 대중교통은 매우 훌륭하다.", e:"Seoul’s public transportation is great."},
            {d:77, s:"교재4", k:"한국 경제가 좋지 않은 상황이다.", e:"The Korean economy is in bad shape."},
            {d:77, s:"대표", k:"도착하려면 아직 좀 남았어요.", e:"We are not there yet."},

            // Day 078
            {d:78, s:"교재1", k:"내가 지갑을 안 가지고 온 듯. 계산 좀 부탁해도 될까?", e:"I think I forgot my wallet. Could you take care of this?"},
            {d:78, s:"교재1", k:"집에 열쇠를 두고 왔어.", e:"I forgot my keys at home."},
            {d:78, s:"교재1", k:"휴대폰을 안 가지고 왔네. 다시 가서 가져올 시간 될까?", e:"I forgot my phone. Do you think I have enough time to go back and get it?"},
            {d:78, s:"교재1", k:"(영화 <나 홀로 집에서> 중에서) Kevin을 집에 두고 왔어요!", e:"We forgot Kevin!"},
            {d:78, s:"교재1", k:"저는 점심 먹고 약 먹는 걸 항상 깜박해요.", e:"I always forget to take my pills after lunch."},
            {d:78, s:"교재4", k:"여자들한테 잘 보이려고 불어를 좀 배워 봤는데, 잘 안되더군요.", e:"I tried learning French to impress women, but that didn’t work."},
            {d:78, s:"교재4", k:"제가 불어를 나름 열심히 배워 봤는데, 발음조차 제대로 할 수 없더라고요.", e:"I tried to learn French, but I couldn’t even get the pronunciation right."},
            {d:78, s:"교재4", k:"연료가 간당간당한데, 그래도 뭐 부산까지 한번 달려 보지 뭐.", e:"I am low on gas, but I’m going to just try and make it all the way to Busan."},
            {d:78, s:"대표", k:"휴대폰 충전기를 회사에 놔두고 왔네.", e:"I forgot my phone charger at work."},

            // Day 079
            {d:79, s:"교재1", k:"너무 춥다. 이런 날씨는 또 처음이군.", e:"It’s super cold. I have never seen any weather like this."},
            {d:79, s:"교재1", k:"이른 오후 시간에는 보통 (교통) 상황이 이렇지는 않은데.", e:"Things aren’t normally like this so early in the afternoon."},
            {d:79, s:"교재1", k:"이 정도로 유려한 디자인은 본 적이 없습니다. 공상 과학 영화에서 바로 나온 것 같은 차네요.", e:"I’ve never seen a sleek design like this. This car looks like it came straight out of a sci-fi movie."},
            {d:79, s:"교재1", k:"상황이 이런 적은 처음입니다.", e:"Things have never been like this."},
            {d:79, s:"교재1", k:"이렇게 맛있는 디저트는 처음이야. 이게 이름이 뭐라고?", e:"I’ve never tasted any dessert like this. What did you call it?"},
            {d:79, s:"교재4", k:"수진이가 다음 달에 결혼한다고 내가 말했던가?", e:"Did I mention that Sujin is getting married next month?"},
            {d:79, s:"교재4", k:"내가 이사한다고 얘기했던가?", e:"Did I mention that I am moving?"},
            {d:79, s:"교재4", k:"지난번에 오늘 저녁 식사 같이 못 한다고 말한다는 걸 깜박했어.", e:"I forgot to mention last time that I won’t be able to join you for dinner tonight."},
            {d:79, s:"교재4", k:"메뉴에 세금이 포함되어 있지 않다고 나와 있지 않네요.", e:"The menu doesn’t mention the tax isn’t included."},
            {d:79, s:"대표", k:"이런 경우는 또 처음 보네요.", e:"I have never seen anything like this."},

            // Day 080
            {d:80, s:"교재1", k:"나 지금 스타벅스에서 커피 사고 있어.", e:"I am at Starbucks, grabbing some coffee."},
            {d:80, s:"교재1", k:"제 상사께서 공항에 부사장님 마중 나가고 안 계십니다.", e:"My boss is out, picking up the VP from the airport."},
            {d:80, s:"교재1", k:"네가 전화했을 때, 거실에서 <에밀리 파리에 가다>를 보고 있었어.", e:"I was in the living room, watching Emily in Paris when you called"},
            {d:80, s:"교재1", k:"저희가 술집에서 위스키를 마시고 있는데 싸움이 벌어졌어요.", e:"We were at the bar, drinking some whisky when a fight broke out."},
            {d:80, s:"교재1", k:"저는 청담에 있는 BMW 전시장에서 SUV 신차를 보고 있었습니다.", e:"I was at the BMW dealership in Cheongdam, checking out the new SUV."},
            {d:80, s:"교재4", k:"입구 쪽에서 보자.", e:"I will see you at the entrance."},
            {d:80, s:"교재4", k:"H 마트는 코리아타운 입구 쪽에 위치한다.", e:"H Mart is located at the entrance of Koreatown."},
            {d:80, s:"교재4", k:"역삼역 3번 출구에서 보는 게 어때?", e:"How about we meet at Yeoksam Station, exit 3?"},
            {d:80, s:"교재4", k:"네가 하루 종일 책상에 앉아서 일을 하니까 허리가 아픈 걸 거야.", e:"Your back is probably sore from sitting at a desk all day."},
            {d:80, s:"대표", k:"나 저녁 사가려고 한솥도시락에 있어.", e:"I am at Hansot, picking up dinner."},

            // Day 081
            {d:81, s:"교재1", k:"선생님 생일 선물로 이 비타민을 해 드리면 어떨까 하는데.", e:"I thought maybe we could get our teacher these vitamins for her birthday."},
            {d:81, s:"교재1", k:"직접 만나지 말고 줌으로 회의하면 어떨까 싶습니다.", e:"I thought maybe we could have a Zoom meeting instead of one in person."},
            {d:81, s:"교재1", k:"이번 여행 때 기차표를 대량 구매하지 말고 버스 전세를 내면 어떨까 하는데요.", e:"I thought maybe we could rent a bus for our trip, instead of buying a bunch of train tickets."},
            {d:81, s:"교재1", k:"혹시 당신이 주요 고객사에 신입 사원을 소개해 주면 어떨까요?", e:"I thought maybe you could introduce the new hire to our major clients."},
            {d:81, s:"교재1", k:"다음 한 주는 온전히 쉬고, 새해 지나서 상쾌하게 다시 보는 게 어떨까요?", e:"I thought maybe we could take the whole week off next week, so we could come back refreshed after New Year’s."},
            {d:81, s:"교재4", k:"혹시 이 신발 다른 사이즈도 있을까요?", e:"I was wondering if you have these shoes in another size."},
            {d:81, s:"교재4", k:"혹시 추천서 좀 써 주실 수 있을까요?", e:"I was hoping that you could write a letter of recommendation for me."},
            {d:81, s:"교재4", k:"우리 수업을 8월에 시작했으면 어떨까 하는데, 그때 괜찮으신가요?", e:"I was hoping to start our sessions in August. Would that work for you?"},
            {d:81, s:"교재4", k:"영업시간 이후라는 건 아는데, 혹시 8시 15분에 매장에 잠깐 들러도 괜찮을까요?", e:"I know it’s just after your hours, but would it be alright if I swung by the store at 8:15?"},
            {d:81, s:"대표", k:"그냥 식당 예약한 것 취소하고 집에 있으면 어떨까 하는데.", e:"I thought maybe we could cancel the reservation and just stay at home."},

            // Day 082
            {d:82, s:"교재1", k:"저희가 인테리어를 다 바꾸었는데도, 분위기가 그대로네요.", e:"We remodeled the whole interior, but that still didn’t change the atmosphere."},
            {d:82, s:"교재1", k:"셔츠를 안에 넣어 입으면 내가 좀 날씬해 보이니?", e:"Does it make me look skinnier when I tuck in my shirt?"},
            {d:82, s:"교재1", k:"당신이 나한테 거짓말하면 나 너무 속상해.", e:"It really hurts my feelings when you lie to me."},
            {d:82, s:"교재1", k:"줌 회의에서는 누군가 말을 하면 그 사람이 큰 화면에 뜹니다.", e:"When someone is speaking in a Zoom meeting, it puts them on the big screen."},
            {d:82, s:"교재4", k:"Harry Kane 선수가 전반 5분 만에 득점함으로써 팀이 일찌감치 앞서 나가게 되었습니다.", e:"Harry Kane scored 5 minutes into the first half and that put them ahead early on."},
            {d:82, s:"교재4", k:"제가 내리기 전에 사람들이 엘리베이터에 타면 정말 짜증이 나요.", e:"It really bothers me when people get in the elevator before I can get off."},
            {d:82, s:"교재4", k:"늦게 일어나면 하루가 엉망이 됩니다.", e:"It ruins my day when I wake up late."},
            {d:82, s:"대표", k:"진통제를 먹었는데도 두통이 가시질 않아.", e:"I took a painkiller, but that didn’t relieve my migraine."},

            // Day 083
            {d:83, s:"교재1", k:"저희 겨울 컬렉션 제품 보고 싶어 하실 듯해서요.", e:"I thought you would be interested in taking a peek at our winter collection."},
            {d:83, s:"교재1", k:"저희 수업에 관심 있으시면, englishnow.com을 방문해 주세요.", e:"If you’re interested in our classes, please visit www.englishnow.com."},
            {d:83, s:"교재1", k:"저 신용카드 하나 더 안 만들어도 되거든요.", e:"I’m not really interested in getting another credit card."},
            {d:83, s:"교재1", k:"너 항상 AI 이야기 하던데, 이 기사 관심 있을 것 같아서.", e:"Since you’re always talking about AI, I thought you would be interested in this article."},
            {d:83, s:"교재1", k:"공석에 관심 있으시면, 언제든 제게 연락 주세요.", e:"If you’re interested in the opening, feel free to contact me at any time."},
            {d:83, s:"교재4", k:"네가 나에게 상대가 된다고 생각해?", e:"You think you could take me on?"},
            {d:83, s:"교재4", k:"네가 원하지 않는다면 내가 그 프로젝트 맡을게.", e:"I will take on the project if you don’t want to."},
            {d:83, s:"교재4", k:"상점들은 주로 크리스마스 시즌 동안 추가 직원을 채용한다.", e:"Shops often take on extra employees during the Christmas season."},
            {d:83, s:"교재4", k:"그 여자는 남자 친구랑 같이 있으면 성격이 바뀐다.", e:"She takes on a different personality when she is with her boyfriend."},
            {d:83, s:"대표", k:"추가로 보험 하나 더 가입할 생각은 없습니다.", e:"I am not really interested in buying another insurance plan."},

            // Day 084
            {d:84, s:"교재1", k:"잠깐 시간 되세요? 곧 있을 워크숍 관련해서 이야기 좀 하려고요.", e:"Have you got a minute? I need to talk to you about the upcoming workshop."},
            {d:84, s:"교재1", k:"잠깐 시간 되세요? 제 자기소개서 관련해서 간단한 질문이 하나 있어서요.", e:"Have you got a minute? I have a quick question about my cover letter."},
            {d:84, s:"교재1", k:"잠깐 시간 되세요? 히터가 고장이 났는데 고치는 방법을 모르겠어요.", e:"Do you have a minute? My heater isn’t working and I don’t know how to fix it."},
            {d:84, s:"교재1", k:"잠깐 시간 돼? 네가 말한 파일을 못 찾겠어.", e:"Do you have a minute? I can’t find the file you mentioned."},
            {d:84, s:"교재4", k:"잠깐 시간 좀 할애해 줄 수 있을까요?", e:"Do you have a minute to spare?"},
            {d:84, s:"교재4", k:"5분밖에 시간이 안 되네요. 빨리 말씀해 주실 수 있을까요?", e:"I’ve got only five minutes to spare. Can you make it quick?"},
            {d:84, s:"교재4", k:"딱 5분이면 됩니다.", e:"I just need five minutes of your time."},
            {d:84, s:"교재4", k:"잠깐이면 됩니다.", e:"This will only take a minute."},
            {d:84, s:"교재4", k:"시간을 너무 많이 뺏어서 죄송합니다.", e:"Sorry for taking up so much of your time."},
            {d:84, s:"대표", k:"너 잠깐 시간 되니? 내가 작업 중인 로고를 보여주고 싶어서.", e:"Hey, have you got a minute? I’d like to show you the logo I’ve been working on."},

            // Day 085
            {d:85, s:"교재1", k:"너 이 동네 잘 아는 듯하네.", e:"You seem quite familiar with this neighborhood."},
            {d:85, s:"교재1", k:"저는 수년 동안 준비반을 가르쳤기 때문에 오픽에 대해 아주 잘 알고 있습니다.", e:"I am quite familiar with OPIc, having taught preparatory classes for years."},
            {d:85, s:"교재1", k:"그 술집이 밖에서 볼 때는 낯익지 않았는데, 안에 들어가 보니 예전에 한 번 왔었다 싶더군요.", e:"The bar didn’t look familiar from the outside, but when I went in, I realized I had been there before."},
            {d:85, s:"교재1", k:"남산 북쪽으로는 등산을 안 해 봤다고 생각했는데, 막상 와 보니 전에 와 본 걸 알겠네요.", e:"I didn’t think I’d hiked the north side of Namsan before, but now that I’m here, it seems familiar."},
            {d:85, s:"교재4", k:"이 표현을 많이 본 것 같은데, 쓰기는 어렵네. 익숙해지려면 시간이 필요할 것 같아.", e:"I am quite familiar with this expression, but I still have trouble using it. I’m afraid it may take some time to get used to."},
            {d:85, s:"교재4", k:"A: 너 아직 해산물을 잘 못 먹는 듯하네.", e:"It seems like you are still not used to seafood."},
            {d:85, s:"교재4", k:"B: 아무리 노력해도 향이나 냄새가 적응이 안 되네.", e:"I can’t get used to the flavor and the smell."},
            {d:85, s:"대표", k:"명랑 핫도그라고 들어 봤니?", e:"Are you familiar with Myungrang Hotdogs?"},

            // Day 086
            {d:86, s:"교재1", k:"저는 마라탕 안 먹어 봤고 제 여자 친구도 마찬가지예요.", e:"I haven’t tried malatang, and my girlfriend hasn’t, either."},
            {d:86, s:"교재1", k:"저도 (회식에) 못 갈 것 같아요.", e:"I’m not going to be able to make it, either."},
            {d:86, s:"교재1", k:"당신이 다시는 몽골에 안 가고 싶다니 다행이다. 나도 가고 싶지 않거든.", e:"I’m glad you never want to go back to Mongolia. I don’t want to, either."},
            {d:86, s:"교재1", k:"애플 역시 신제품 출시 계획이 없습니다.", e:"Apple isn’t planning on releasing a new product, either."},
            {d:86, s:"교재1", k:"내 전화기에서도 그 웹 사이트가 안 열리네.", e:"I can’t pull up the website on my phone, either."},
            {d:86, s:"교재4", k:"전 여자 친구랑 완전히 연락을 끊든지, 아니면 우리 헤어져.", e:"Either you completely cut off your ex-girlfriend, or we’re breaking up."},
            {d:86, s:"교재4", k:"민박집을 찾아 보던지 아니면 캠팅장 자동차 뒤 칸에서 자야지 뭐.", e:"Either we try and find a guesthouse, or we just sleep in the back of the car at a campsite."},
            {d:86, s:"교재4", k:"정시 출근을 하든지, 아니면 다른 직장을 알아보게나.", e:"Either you show up on time, or you start looking for another job."},
            {d:86, s:"대표", k:"나도 용리단길 안 가 봤는데.", e:"I haven’t been to Yongnidangil, either."},

            // Day 087
            {d:87, s:"교재1", k:"다른 진공청소기와 달리, V4는 몇 분이면 충전할 수 있습니다.", e:"Unlike other vacuum cleaners, the V4 can charge in a matter of minutes."},
            {d:87, s:"교재1", k:"다른 대도시와는 달리, 세종시는 매우 깨끗합니다. 잘 계획되어서 그런가 봅니다.", e:"Unlike other big cities, Sejong is pretty clean. I think that’s because it was planned out so well."},
            {d:87, s:"교재1", k:"이 유튜브 채널은 다른 채널들과 달리, 경제에 관해 꽤 심도 있는 내용을 다뤄.", e:"This YouTube channel goes pretty in-depth into financial topics, unlike other channels."},
            {d:87, s:"교재1", k:"이전 선생님들과는 달리, James는 도움 되는 피드백을 주십니다.", e:"James gives me useful feedback, unlike my previous teachers."},
            {d:87, s:"교재4", k:"그는 전에 내가 함께했던 어떤 선생님과도 다르다.", e:"He is unlike any other teacher I’ve had before."},
            {d:87, s:"교재4", k:"트럼프는 우리가 경험한 기존 대통령들과는 다르다.", e:"Trump is unlike any other president we’ve had."},
            {d:87, s:"교재4", k:"이것은 당신이 전에 본 어떤 현상과도 다르다.", e:"This is unlike anything you have ever seen before."},
            {d:87, s:"대표", k:"다른 영업직과는 달리, 저는 기본급이 있습니다.", e:"I get a regular salary, unlike other salespeople."},

            // Day 088
            {d:88, s:"교재1", k:"이상하게 사람이 그렇게 많지 않네. 보통 주말이면 백화점에 사람 엄청 많은데.", e:"It’s unusual that there aren’t many people here. Department stores are usually super crowded on weekends."},
            {d:88, s:"교재1", k:"남자라고 손톱 다듬지 말라는 법은 없지.", e:"There is nothing unusual about a guy getting his nails done."},
            {d:88, s:"교재1", k:"제가 하는 업무의 경우 야근이 일상입니다.", e:"Working overtime is not unusual in my job."},
            {d:88, s:"교재1", k:"10월치고는 너무 춥습니다.", e:"It’s unusually cold for October."},
            {d:88, s:"교재1", k:"뉴욕시의 24시간 대중교통 시스템은 미국 도시로서는 이례적입니다.", e:"New York City’s 24-hour public transit system is unusual for an American city."},
            {d:88, s:"교재4", k:"저희 회사는 복지가 상당히 좋아요.", e:"My company offers nice benefits and perks."},
            {d:88, s:"교재4", k:"나 SNS 완전히 끊을까 해. 끊임없이 다른 사람이랑 비교하게 되니까 건강에 안 좋아.", e:"I’m thinking about quitting social media for good. I’m constantly comparing myself to others, and that’s not healthy."},
            {d:88, s:"교재4", k:"저는 특별한 날에만 이 안경을 씁니다.", e:"I only wear these glasses on special occasions."},
            {d:88, s:"대표", k:"미국은 병원 입원비가 너무 지나치게 비싸요.", e:"In America, hospital stays are unusually expensive."},

            // Day 089
            {d:89, s:"교재1", k:"점심 내가 살게. 그렇게 하자고.", e:"Let me treat you to lunch. I insist."},
            {d:89, s:"교재1", k:"(아빠가 자취하는 딸에게) 이 돈 받아. 그렇게 해!", e:"Take this money. I insist!"},
            {d:89, s:"교재1", k:"제 아내가 벽지 색상으로 흰색을 고집하더군요.", e:"My wife insisted on white for the wall."},
            {d:89, s:"교재1", k:"숙박에 있어서 만큼은, 저희 남편은 늘 비싼 곳만 고집해요.", e:"When it comes to accommodations, my husband always insists on the expensive option."},
            {d:89, s:"교재1", k:"괜히 폐 끼치고 싶지는 않지만, 정 원한다면 너희들이랑 하루 더 있을게.", e:"I don’t really want to outstay my welcome, but we will stay one more day with you guys, if you insist."},
            {d:89, s:"교재4", k:"Jeff는 둘이 그냥 친구라고 주장하지만, 그 둘의 문자 메시지는 나에게 보여 주려고 하지 않는다니까.", e:"Jeff insists that they are just friends, but he doesn’t let me see their text messages."},
            {d:89, s:"교재4", k:"그는 자신의 가방에 어떻게 마약이 들어갔는지 전혀 모르겠다고 주장한다.", e:"He insists that he has no idea how the drugs got into his luggage."},
            {d:89, s:"교재4", k:"다음 대금 결제는 꼭 늦지 않게 해 주시기 바랍니다.", e:"We insist that the next payment be made on time."},
            {d:89, s:"교재4", k:"육고기가 몸에 안 좋다는 건 이제는 거의 확실해진 것 같은데도, 우리 엄마는 내가 집에 갈 때마다 육고기를 먹으라고 한다.", e:"I think it’s pretty clear by now that red meat isn’t good for you, but my mom insists that we have some every time I come over."},
            {d:89, s:"대표", k:"제가 차로 집까지 모셔다 드린다니까요. 그렇게 하시죠.", e:"Please let me drive you home. I insist."},

            // Day 090
            {d:90, s:"교재1", k:"네가 말한 빵집이 사실 나 일하는 곳 근처에 있어.", e:"The bakery you mentioned is actually close to where I work."},
            {d:90, s:"교재1", k:"이 영화에 많은 노력이 들어갔다는 점을 관객들이 알아줬으면 좋겠네요.", e:"I wish the audience could see what really went into this movie."},
            {d:90, s:"교재1", k:"제 연봉이 궁금하세요?", e:"Do you want to know how much I make?"},
            {d:90, s:"교재1", k:"이 커피는 서울에서 흔히 볼 수 있는 것과는 많이 달라.", e:"This coffee is so different from what you normally find in Seoul."},
            {d:90, s:"교재1", k:"Jorge가 몇 번이나 설명해 주긴 했는데도 그 친구 직업을 잘 모르겠어.", e:"Jorge has explained it many times, but I’m still not really sure what he does for a living."},
            {d:90, s:"교재4", k:"사람들이 카페에서 일을 할 수 있다는 게 이해가 안 돼. 집중을 방해하는 게 너무 많잖아.", e:"I don’t understand how people can get any work done at cafes. There are too many distractions."},
            {d:90, s:"교재4", k:"우리 집 고양이가 뭘 찢기 시작하면 집중이 안 됩니다.", e:"I get distracted when my cat starts tearing things up."},
            {d:90, s:"교재4", k:"제발 다리 좀 떨지 말아 줄래? 집중이 안 돼.", e:"Can you please stop tapping your foot already? It’s so distracting"},
            {d:90, s:"대표", k:"여기 거실에 해 두신 것이 마음에 듭니다.", e:"I like what you have going on here in the living room."},

            // Day 091
            {d:91, s:"교재1", k:"그 사람은 키에 비해 어깨가 넓은 편이야.", e:"He has relatively wide shoulders for his height."},
            {d:91, s:"교재1", k:"사마귀는 수명이 3년인데, 곤충 세계에서는 그나마 오래 사는 편이지.", e:"The mantis can live for three years, which is a relatively long time in the insect kingdom."},
            {d:91, s:"교재1", k:"오미크론으로 사망하는 사람이 있기는 해도, 대부분의 경우 증상이 그렇게 심하지는 않습니다.", e:"Some people still die from Omicron, but for most, symptoms are relatively mild."},
            {d:91, s:"교재1", k:"온수기 고치는 데 이백 달러는 좀 비싸다 싶어. 그래도 새것 사는 비용에 비하면 저렴한 편이지.", e:"$200 sounds like a lot of money to fix the water heater, but it’s relatively small compared to the cost of buying a whole new one."},
            {d:91, s:"교재4", k:"새해 연휴 이후에 체중이 너무 많이 늘었다.", e:"I gained too much weight after the New Year’s holiday."},
            {d:91, s:"교재4", k:"자신감을 얻을 수 있는 좋은 방법이 있다면 알려 주세요.", e:"If there is a good way to gain confidence, please let me know."},
            {d:91, s:"교재4", k:"그 밴드는 버스킹 허가증을 받지 못했다.", e:"The band didn’t obtain a permit to do their busking."},
            {d:91, s:"교재4", k:"나는 코넬 대학에서 학위를 받았다.", e:"I obtained a degree from Cornell."},
            {d:91, s:"대표", k:"천만 원이면 중고차치고는 그나마 저렴한 편이긴 한데, 그래도 저한테는 비싸요.", e:"10 million won is relatively cheap for a used car, but it’s still expensive for me."},

            // Day 092
            {d:92, s:"교재1", k:"어젯밤에 너무 마셨더니 숙취가 너무 심하네.", e:"I have a terrible hangover from drinking too much last night."},
            {d:92, s:"교재1", k:"지난주에 매일 운동을 해서 그런지 근육이 당기는 것 같아.", e:"I think I got this muscle tightness from working out every day last week."},
            {d:92, s:"교재1", k:"뭐 대단한 거 하다가 상처가 생긴 게 아니고요. TV 전선 줄에 걸려 넘어졌어요.", e:"I didn’t get this scar from anything exciting. I just tripped on the TV’s power cord."},
            {d:92, s:"교재1", k:"축구하다가 멍이 든 것 같아요.", e:"I think I got this bruise from playing soccer."},
            {d:92, s:"교재1", k:"너무 오래 컴퓨터 화면을 봤더니 편두통이 생겼어.", e:"I got this migraine from staring at my computer screen too long."},
            {d:92, s:"교재4", k:"제가 서류를 이메일로 보내겠습니다.", e:"I will email you the documents."},
            {d:92, s:"교재4", k:"어젯밤에 밖에서 파티하고 놀았어.", e:"I was out partying last night."},
            {d:92, s:"교재4", k:"드디어 초콜릿 칩 쿠키 레시피를 완성했어", e:"I finally perfected my recipe for chocolate chip cookies."},
            {d:92, s:"교재4", k:"그 신제품은 소매가가 만 오천 원이다.", e:"The new product retails for 15,000 won."},
            {d:92, s:"대표", k:"하루 종일 책상에 앉아 있어서 허리 통증이 생긴 것 같아.", e:"I think I got this back pain from sitting at a desk all day."},

            // Day 093
            {d:93, s:"교재1", k:"그녀는 시간에 쫓기면 예민해져.", e:"She gets irritable when she feels pressed for time."},
            {d:93, s:"교재1", k:"Tom이 도착하면 파티가 재미있어져.", e:"Parties become more interesting when Tom arrives."},
            {d:93, s:"교재1", k:"저도 실수를 덜 하고 싶은데, 말을 빨리해야 한다는 압박감이 들면 그게 어려워요.", e:"I want to make fewer mistakes, but it’s hard when I am under pressure to speak quickly."},
            {d:93, s:"교재1", k:"저는 제 친구들의 말을 잘 들어 주려고 노력합니다. 근데 제가 어려움이 있을 때는 아무도 제 말을 귀담아들으려 하는 것 같지가 않아요.", e:"I try to be a good listener for my friends, but it seems like no one is willing to listen to me when I have problems."},
            {d:93, s:"교재4", k:"털을 짧게 깎아 놓으니 네 개가 뭔가 어색해 보인다.", e:"Your dog doesn’t look right with short fur."},
            {d:93, s:"교재4", k:"이 신발 뭔가 불편해.", e:"Something about these shoes doesn’t feel right."},
            {d:93, s:"교재4", k:"에어컨에 뭔가 문제가 있는 것이 틀림없어. 소리가 이상해. 따뜻한 바람만 나오고.", e:"Something must be wrong with the air conditioner. It doesn’t sound right, and it’s only blowing warm air."},
            {d:93, s:"교재4", k:"고든 램지가 영상에서 한 것처럼 오믈렛을 만들어 보려 했지만, 맛이 뭔가 이상했다.", e:"I tried to cook an omelette just like Gordon Ramsay did in the video, but it didn’t taste quite right."},
            {d:93, s:"대표", k:"비 오면 머리가 엉망이 돼요.", e:"My hair gets really messy when it rains."},

            // Day 094
            {d:94, s:"교재1", k:"이번 주말에 등산 갈 생각이에요.", e:"I’m planning on going hiking this weekend."},
            {d:94, s:"교재1", k:"연휴 때 경주 갈까 해.", e:"I’m planning on going to Gyeongju for the holiday."},
            {d:94, s:"교재1", k:"너 내일 여자 친구 만날 거야?", e:"Are you planning on seeing your girlfriend tomorrow?"},
            {d:94, s:"교재1", k:"오늘 수영장에서 배영을 연습할 생각이야.", e:"Today, at the pool, I’m planning on working on my backstroke."},
            {d:94, s:"교재1", k:"삼성이 텍사스 타일러에 공장을 세울 계획입니다.", e:"Samsung is planning on building a factory in Tyler, Texas."},
            {d:94, s:"교재4", k:"부인 생일 선물을 뭐 해줬어?", e:"What did you get your wife for her birthday?"},
            {d:94, s:"교재4", k:"주말을 맞아 무슨 계획 있어?", e:"Do you have any plans for the weekend?"},
            {d:94, s:"교재4", k:"크리스마스에 꼭 뭔가 특별한 걸 해야 해?", e:"Do we have to do something special on Christmas?"},
            {d:94, s:"대표", k:"크리스마스를 맞아 이번 주말에 부산에 가려고 해요.", e:"We’re planning on going to Busan this weekend for Christmas."},

            // Day 095
            {d:95, s:"교재1", k:"미안한데 나 20분 늦어.", e:"I am afraid I am running 20 minutes late."},
            {d:95, s:"교재1", k:"이 가격이 제 예산 범위를 조금 초과하네요.", e:"I’m afraid this price is a bit beyond my budget."},
            {d:95, s:"교재1", k:"죄송한데 오늘 밤에는 선약이 있어요.", e:"I’m afraid I already have plans tonight."},
            {d:95, s:"교재1", k:"죄송한데 그 점은 동의하기 힘듭니다.", e:"I’m afraid I have to disagree with you on that."},
            {d:95, s:"교재1", k:"(회의에서) 죄송한데 조금 일찍 일어나 볼게요. 법무팀에서 급한 전화가 오기로 되어 있어서요.", e:"I’m afraid I may have to leave the meeting early. I’m expecting an urgent call from our legal team."},
            {d:95, s:"교재4", k:"나 아기 성별을 곧 알게 될 것 같아.", e:"I will find out the baby’s gender soon."},
            {d:95, s:"교재4", k:"카페에 거의 다 왔어. 곧 도착해.", e:"I’m almost to the cafe. I’ll be there any minute."},
            {d:95, s:"교재4", k:"안전벨트를 착용해 주십시오. 곧 이륙할 예정입니다.", e:"Please fasten your seat belts. The plane will be taking off shortly."},
            {d:95, s:"대표", k:"죄송한데 찾으시는 제품이 재고가 없습니다.", e:"I’m afraid the item you’re looking for is out of stock."},

            // Day 096
            {d:96, s:"교재1", k:"저는 데이트가 없으면 세차를 안 해요.", e:"I don’t wash my car unless I have a date coming up."},
            {d:96, s:"교재1", k:"저는 마감 기한이 없으면 빈둥거리게 됩니다.", e:"I tend to mess around unless I have a deadline."},
            {d:96, s:"교재1", k:"저는 술기운이 좀 알딸딸하게 올라와야 모르는 사람에게 말을 걸 수 있어요.", e:"I can’t talk to strangers unless I’m a little tipsy."},
            {d:96, s:"교재1", k:"마스크 안 쓰면 가게에 못 들어갑니다.", e:"You can’t go into stores unless you wear a mask."},
            {d:96, s:"교재1", k:"전념하지 않으면, 가시적인 성과를 낼 수 없어요.", e:"Unless you’re committed, you won’t make real progress."},
            {d:96, s:"교재4", k:"제 택배 언제 도착할까요?", e:"When will my package arrive?"},
            {d:96, s:"교재4", k:"아마 내일쯤 도착할 것 같습니다.", e:"It should probably arrive sometime tomorrow."},
            {d:96, s:"교재4", k:"저녁 식사가 20분 뒤면 준비될 거예요.", e:"Dinner will be ready in 20 minutes."},
            {d:96, s:"교재4", k:"20분 후면 준비되겠죠.", e:"Dinner should be ready in 20 minutes."},
            {d:96, s:"대표", k:"요즘은 꼭 필요한 경우가 아니면 차 안 가지고 다녀요.", e:"I don’t drive nowadays unless I really have to."},

            // Day 097
            {d:97, s:"교재1", k:"근데 오늘 너무 피곤하네.", e:"I’m really tired today, though."},
            {d:97, s:"교재1", k:"그런데 오늘 밤엔 헬스장 갈 수 있을지 모르겠네요.", e:"I’m not sure if I can make it to the gym tonight, though."},
            {d:97, s:"교재1", k:"근데 나 그렇게 배고프지는 않아.", e:"I’m not particularly hungry, though."},
            {d:97, s:"교재1", k:"그런데 몸이 좀 안 좋아요.", e:"I’m not feeling well, though."},
            {d:97, s:"교재1", k:"근데 나가고 싶은 마음이 안 생겨.", e:"I’m not in the mood to go out, though."},
            {d:97, s:"교재4", k:"여기 자두 받으렴! 오다가 자두 나무를 봤거든.", e:"Here, take a plum! I came across a plum tree on my way."},
            {d:97, s:"교재4", k:"방금 이 뉴스 기사를 발견했어. 보니까 네가 관심 있을 만한 내용이네.", e:"I just came across this news article, and it seemed like something you’d be interested in."},
            {d:97, s:"교재4", k:"마포에서 멋진 이발소를 찾았어. 내 친구들 모두에게 추천해주고 있어.", e:"I came across this great barber in Mapo, and I’m recommending him to all my friends."},
            {d:97, s:"대표", k:"가격은 너무 좋네요. 근데 제가 이미 괜찮은 마사지 의자가 있거든요.", e:"That is a great price. I’m afraid I already have a decent massage chair, though."},

            // Day 098
            {d:98, s:"교재1", k:"역기 들기가 어떻게 취미가 되니? 도저히 이해가 안 돼.", e:"How can weight lifting be a hobby? That’s never made sense to me."},
            {d:98, s:"교재1", k:"이제야 스토리가 이해되네. 주인공이 처음부터 암살자였던 거네.", e:"Now it makes sense. The main character has actually been an assassin the whole time."},
            {d:98, s:"교재1", k:"그 친구는 여기 6개월 계약으로 있는 거니까, 차를 빌리는 게 더 낫겠다.", e:"It makes better sense for him to rent a car, since he’s just here on a six-month contract."},
            {d:98, s:"교재1", k:"네가 아무리 나한테 인플레이션의 개념을 설명하려고 해도, 전혀 이해가 안 돼.", e:"No matter how many times you try to explain inflation to me, it never makes sense."},
            {d:98, s:"교재4", k:"장인어른과 대화할 때마다 늘 정치 이야기가 나와서 싸우게 됩니다.", e:"Every time I talk to my father-in-law, politics comes up and we get into a fight."},
            {d:98, s:"교재4", k:"블루투스 스피커에 연결하려고 하면 가끔 이런 에러 메시지가 뜹니다.", e:"This error message sometimes comes up when I try to connect to my Bluetooth speaker."},
            {d:98, s:"교재4", k:"갑자기 중요한 일이 생겨서요. 오늘 수업은 못 갈 것 같습니다.", e:"Something important just came up. I’m afraid I can’t make it to class today."},
            {d:98, s:"대표", k:"민수는 머리를 삼만 삼천 원 주고 자른대. 난 도저히 이해가 안 가.", e:"Minsu said he spends 33,000 won on his haircut. That doesn’t make sense to me."},

            // Day 099
            {d:99, s:"교재1", k:"백화점 들를 시간은 안 될 것 같아. 콘서트가 45분 후에 시작하잖아.", e:"I don’t think we have time to swing by the department store. The concert starts in 45 minutes."},
            {d:99, s:"교재1", k:"그 둘은 썩 잘 어울리지는 않을 듯해.", e:"I don’t think they would be quite right for each other."},
            {d:99, s:"교재1", k:"택시를 타도 제시간에 도착하는 건 힘들 것 같아요.", e:"I don’t think there’s any way we can make it on time, even if we take a taxi."},
            {d:99, s:"교재1", k:"그 테이블이 우리 집 거실에는 안 맞지 싶은데.", e:"I don’t think that table would fit in our living room."},
            {d:99, s:"교재1", k:"그렇게 빠듯한 예산으로 프로젝트를 완료하기는 어려울 듯해요.", e:"I don’t think we can complete the project on such a tight budget."},
            {d:99, s:"교재4", k:"이제 다음으로 넘어가도 될 것 같으니 다음 슬라이드를 띄울게요.", e:"I think we’re ready to move on, so I’ll just pull up the next slide."},
            {d:99, s:"교재4", k:"질문에 대한 답을 모르겠으면, 넘어가고 쉬운 문제들부터 풀고 난 다음 다시 풀어 봐.", e:"When you don’t know the answer to a question, move on and then try again after finishing the easier ones."},
            {d:99, s:"교재4", k:"너 아직 Sarah에 대한 감정이 남아있는 거야? 벌써 4개월이나 됐잖아. 이젠 잊고 새 출발 해야지.", e:"Are you still upset about Sarah? It’s been four months already. It’s time to move on."},
            {d:99, s:"대표", k:"그 둘은 결국 잘 안 될 거야.", e:"I don’t think things are gonna work out between them."},

            // Day 100
            {d:100, s:"교재1", k:"그 사람이 나한테 관심이 있는 건지 잘 모르겠어.", e:"I’m not quite sure if he is interested in me."},
            {d:100, s:"교재1", k:"오늘 너희들이랑 저녁 식사 같이 할 수 있을지 잘 모르겠어.", e:"I’m not quite sure if I can join you for dinner."},
            {d:100, s:"교재1", k:"그걸 영어로 어떻게 표현하는지 잘 모르겠어요.", e:"I’m not quite sure how to say that in English."},
            {d:100, s:"교재1", k:"그녀가 왜 나한테 화가 난 건지 잘 모르겠어.", e:"I’m not quite sure why she is mad at me."},
            {d:100, s:"교재1", k:"몇 시에 퇴근할 수 있을지 잘 모르겠어.", e:"I’m not quite sure when I can get off work."},
            {d:100, s:"교재4", k:"그 집 영업시간을 모르겠네. 전화해서 물어보자.", e:"I’m not sure about their hours, so let’s call and ask."},
            {d:100, s:"교재4", k:"나 비자 발급 요건을 정말 잘 모르겠어.", e:"I am not really sure about the visa requirements."},
            {d:100, s:"교재4", k:"너 그 사람 진짜 믿을 수 있다고 확신해?", e:"Are you sure you can really trust him?"},
            {d:100, s:"교재4", k:"저희가 세부적인 내용에 대해서는 잘 모릅니다.", e:"We are not certain about the details."},
            {d:100, s:"교재4", k:"다음 모델이 언제 출시될지는 잘 모르겠습니다.", e:"We are not certain when the next model will come out."},
            {d:100, s:"대표", k:"내가 제대로 하고 있는 건지 잘 모르겠어.", e:"I’m not quite sure if I am doing it right."}
        ];

        allData.forEach((item, index) => item.id = index + 1);
        const sourceMap = { 
            "대표": "대표예제", 
            "교재1": "Model examples", 
            "교재2": "Small talk", 
            "교재3": "Cases in point", 
            "교재4": "Further studies" 
        };

        let selectedWeekMode = 'w16';
        let quizPool = [];
        let currentIndex = 0;
        let starredIds = new Set();

        function selectWeek(mode) {
            selectedWeekMode = mode;
            document.querySelectorAll('.week-tab').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(`tab-${mode}`).classList.add('active-tab');
            
            const titleEl = document.getElementById('selected-week-title');
            const titles = {
                'w16': 'DAY 76-80', 'w17': 'DAY 81-85', 'w18': 'DAY 86-90',
                'w19': 'DAY 91-95', 'w20': 'DAY 96-100', 'total': 'DAY 76-100 전체'
            };
            titleEl.innerText = titles[mode];
        }

        function unlockAudio() {
            if ('speechSynthesis' in window) {
                const msg = new SpeechSynthesisUtterance("");
                window.speechSynthesis.speak(msg);
            }
        }

        function startQuiz(difficulty) {
            unlockAudio();
            let filtered = [];
            
            if (selectedWeekMode === 'total') {
                filtered = allData.filter(item => item.d >= 76 && item.d <= 100);
            } else {
                // w16: 76-80, w17: 81-85 ...
                const weekNum = parseInt(selectedWeekMode.replace('w', ''));
                const startDay = (weekNum - 16) * 5 + 76;
                const endDay = startDay + 4;
                filtered = allData.filter(item => item.d >= startDay && item.d <= endDay);
            }

            if (difficulty === 'mild') {
                filtered = filtered.filter(item => item.s === '대표' || item.s === '교재1');
            }

            if (filtered.length === 0) {
                alert("해당 범위에 데이터가 없습니다.");
                return;
            }

            quizPool = filtered.sort(() => Math.random() - 0.5).slice(0, 10);
            currentIndex = 0;
            starredIds.clear();
            
            document.getElementById('setup-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            showQuestion();
        }

        function showQuestion() {
            const item = quizPool[currentIndex];
            const card = document.getElementById('card-container');
            card.classList.remove('flipped');
            document.getElementById('next-btn').classList.add('hidden');
            document.getElementById('star-btn').classList.remove('star-checked');
            
            document.getElementById('q-korean').innerText = item.k;
            document.getElementById('q-english').innerText = item.e;
            document.getElementById('q-info').innerText = `Day ${item.d} - ${sourceMap[item.s]}`;
            document.getElementById('progress-text').innerText = `${currentIndex + 1} / ${quizPool.length}`;
            
            if (starredIds.has(item.id)) document.getElementById('star-btn').classList.add('star-checked');
        }

        function flipCard() {
            document.getElementById('card-container').classList.add('flipped');
            document.getElementById('next-btn').classList.remove('hidden');
        }

        function toggleStar(e) {
            e.stopPropagation();
            const item = quizPool[currentIndex];
            const starBtn = document.getElementById('star-btn');
            if (starredIds.has(item.id)) {
                starredIds.delete(item.id);
                starBtn.classList.remove('star-checked');
            } else {
                starredIds.add(item.id);
                starBtn.classList.add('star-checked');
            }
        }

        function playTTS() {
            const text = document.getElementById('q-english').innerText;
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                const msg = new SpeechSynthesisUtterance(text);
                msg.lang = 'en-US';
                msg.rate = 0.9;
                window.speechSynthesis.speak(msg);
            }
        }

        function nextQuestion() {
            currentIndex++;
            if (currentIndex < quizPool.length) showQuestion();
            else showResults();
        }

        function showResults() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            const listEl = document.getElementById('starred-list');
            listEl.innerHTML = '<p class="text-[10px] text-slate-400 font-black mb-4 uppercase tracking-widest text-center">⭐ 체크한 문장 다시보기</p>';
            
            const starredItems = quizPool.filter(item => starredIds.has(item.id));
            if (starredItems.length === 0) {
                listEl.innerHTML += '<p class="text-sm text-slate-400 py-12 text-center font-bold">오답이 없습니다! 완벽해요.</p>';
            } else {
                starredItems.forEach(item => {
                    const div = document.createElement('div');
                    div.className = "p-5 bg-indigo-50 rounded-2xl border border-indigo-100 mb-3";
                    div.innerHTML = `
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-[9px] bg-indigo-500 text-white px-1.5 py-0.5 rounded font-black">DAY ${item.d}</span>
                            <span class="text-[9px] text-indigo-400 font-bold italic">${sourceMap[item.s]}</span>
                        </div>
                        <p class="text-xs text-slate-500 mb-1">${item.k}</p>
                        <p class="text-sm font-black text-indigo-900">${item.e}</p>
                    `;
                    listEl.appendChild(div);
                });
            }
        }
    </script>
</body>
</html>
