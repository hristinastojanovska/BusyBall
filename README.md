
# Busy Ball

„Busy Ball” е игра со којашто е опфатен животот на едно зафатено топче. Целта е да му се помогне со извршување на обврските што ги има во текот на денот.

При вклучување на играта на екранот се појавува почетната сцена каде топчето спие и по неколку секунди започнува да ѕвони алармот за да го разбуди. Исто така, од страна има To Do List со четири таскови, кои, всушност, претставуваат mini games и на почетокот се заклучени. 

![App Screenshot](https://snipboard.io/lLEW90.jpg )

## Mini Games

- Quiz
- Nonogram
- Sliding puzzle
- Car racing game

При клик на алармот се појавува форма кадешто за време од две минути треба да се решат три математички задачи кои се потребни за отклучување на тасковите. 
- Доколку само една математичка задача се реши точно, се отклучува само првиот таск. 
- Со точно решени две задачи се отклучуваат првите два таскови 
- Со решавање на сите задачи се отклучуваат три таскови. 
- На листата останува уште еден неотклучен таск, кој може да се пристапи само со успешно завршување на првиот таск. 
По завршување со математичките задачи, на главната форма топчето „се буди“ и може да се продолжи со извршување на отклучените таскови.

![App Screenshot](https://snipboard.io/KDslhP.jpg)

## Првиот таск „studying“ (Quiz):

Една од обврските на топчето е да учи и поради тоа првиот таск е квиз од 10 прашања. Се смета дека топчето има научено доколку 7 или повеќе прашања се точно одговорени, со што исто така се отклучува и четвртиот таск на листата. 

![App Screenshot](https://snipboard.io/bj6SBR.jpg)

### Имплементација на играта
- Се креира нова форма „Quiz“ 
- Во text box се прикажуваат 10 прашања едно по едно, кои на случаен начин се избираат од dictionary. 
- Има 4 понудени одговори во форма на radio buttons
- Копче „Next“ служи за преминување на следно прашање
- Progress Bar покажува колку прашања се точно одговорени. На почетокот е црвен, но станува зелен откако точно ќе се решат 70% од прашањата.

### Функцијата ShowQuestions(): 

```c#
public void ShowQuestions()
{
    CountQuestions++;
            
    if (CountQuestions <= MaxQuestions)
    {
        lblNumOfQuestion.Text =$"{CountQuestions}/{MaxQuestions}";
        var randomQuestion = Questions.ElementAt(Random.Next(0, Questions.Count));
                
        tbQuestion.Text = randomQuestion.Key;
        rbAnswer1.Text = randomQuestion.Value[0];
        rbAnswer2.Text = randomQuestion.Value[1];
        rbAnswer3.Text = randomQuestion.Value[2];
        rbAnswer4.Text = randomQuestion.Value[3];

        CorrectAnswer = randomQuestion.Value[4];
    }
    else
    {
        DialogResult dg = MessageBox.Show($"Congratulations. You scored: {Score}", "", MessageBoxButtons.OK);
        if (dg == DialogResult.OK)
        {
            this.Close();
            IsClosed = true;
        }

    }
}
```

- Се користи за прикажување на прашање. 
- Променливата CountQuestions служи за броење колку прашања се прикажани на формата. 
- Се проверува дали CountQuestions е помало или еднакво на MaxQuestions чија иницијална вредност е 10. 
- Ако се исполнува условот, со помош на Random() се зима прашање по случаен избор од податочната структура dictionary, во којашто како клуч се чува прашањето, а како вредност имаме низа, каде на првите четири позиции се чуваат понудените одговори, а на последната точниот одговор.  
- Прашањето се печати во text box, а одговорите се прикажуваат со radio buttons. 
- Доколку условот не е исполнет, односно CountQuestions има поголема вредност од 10, се печати MessageBox со бројот на точно одговорени прашања. 
- По исклучување на MessageBox се исклучува квизот.

## Вториот таск „Painting“ (Nonogram):

Овој таск е позната логичка игра „Nonogram“, во која е дадена мрежа составена од ќелии кои треба да се исполнат или останат празни. Целта е да се открие слика скриена зад правилно исполнетите ќелии, врз основа на нумеричките покажувачи за секој ред и колона.

### Како се игра:
- Мрежа: Се започнува со квадратна мрежа, поделена на редови и колони. Мрежата е првично празна, а целта е да се исполнат одредените ќелии за да се открие слика.
- Покажувачи: Низ горниот дел и левата страна на мрежата, се наоѓа низа од броеви. Овие броеви ја претставуваат должината на последователно исполнетите ќелии во секој ред или колона. Редоследот на броевите се соодветствува со редоследот на исполнетите блокови во тој ред или колона.
- Завршување на играта: Со продолжување, постепено ќе почне да се забележува обликот на скриената слика во мрежата. Со точно заклучување и исполнување на ќелиите, на крајот се открива целосната слика во боја.

Нашиот Nonogram  користи мрежа со големина 10x10. Со лев клик на глувчето, ќелијата се бои, а со десен клик на ќелијата се става „X“, доколку сте сигурни дека истата не треба да биде обоена. Со успешно решавање на оваа игричка, таскот „Painting“ се смета за завршен.

![App Screenshot](https://snipboard.io/q5sWaN.jpg)
![App Screenshot](https://snipboard.io/XdsTkc.jpg)

## Третиот таск „Cleaning“ (Sliding puzzle):

„Sliding puzzle“, е класична игра која се игра на квадратна мрежа. 
Мрежата се состои од илустрирани плочки, а целта е да се преаранжираат плочките од почетната измешана состојба во специфична целна состојба со поместување на нив една по една во празното место.
- Може да се поместат само плочките што се наоѓаат околу празното место. 
- За да се помести една плочка, едноставно треба да се кликне врз неа и таа ќе се придвижи на прaзното место. 
- Плочките може да се придвижуваат вертикално или хоризонтално, но не се дозволени дијагонални поместувања.

![App Screenshot](https://snipboard.io/INSdPK.jpg)

## Четвртиот таск „Gaming“ (Car racing game):

По завршување со учење, топчето ќе сака да се одмори, преку играње на игра, каде треба да се трка со автомобили.  
- Пред започнување на играта, мора да се да се избере автомобил со кој играчот би сакал да се трка. Изборот треба да се направи  помеѓу црвениот и синиот автомобил. 
- Автомобилот може да се движи само во лева и десна насока, а тоа се прави со помош на левата и десната стрелка од тастатура, соодветно. Потребно е да се внимава, за да не дојде до судир со други автомобили. 
- При возењето се зголемуваат поените, а за одредено време се зголемува и брзината на движење на автомобилите. 
- Ако се освојат од 200 до 700 поени, играчот добива бронзен медал, од 700 до 1500 се добива сребрен медал, а со освојување над 1500 поени, играчот добива златен медал. 
Таскот „Gaming“ се смета за завршен, доколку со играње ќе биде освоен било каков медал. 

![App Screenshot](https://snipboard.io/IwNn7d.jpg)

## Windows Forms Project by:

- [@ivanabjk](https://www.github.com/ivanabjk)
- [@hristinastojanovska](https://www.github.com/hristinastojanovska)
