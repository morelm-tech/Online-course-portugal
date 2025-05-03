import { useEffect, useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Tabs, TabsList, TabsTrigger, TabsContent } from "@/components/ui/tabs";
import { Video } from "@/components/ui/video";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Textarea } from "@/components/ui/textarea";

export default function PortugalCulturalCourse() {
  const [week, setWeek] = useState("week1");
  const [quizAnswer, setQuizAnswer] = useState("");
  const [quizFeedback, setQuizFeedback] = useState("");
  const [finalAnswer, setFinalAnswer] = useState("");
  const [finalFeedback, setFinalFeedback] = useState("");

  useEffect(() => {
    const savedWeek = localStorage.getItem("selectedWeek");
    const savedQuiz = localStorage.getItem("quizAnswer");
    const savedFinal = localStorage.getItem("finalAnswer");
    if (savedWeek) setWeek(savedWeek);
    if (savedQuiz) setQuizAnswer(savedQuiz);
    if (savedFinal) setFinalAnswer(savedFinal);
  }, []);

  useEffect(() => {
    localStorage.setItem("selectedWeek", week);
  }, [week]);

  useEffect(() => {
    localStorage.setItem("quizAnswer", quizAnswer);
  }, [quizAnswer]);

  useEffect(() => {
    localStorage.setItem("finalAnswer", finalAnswer);
  }, [finalAnswer]);

  const handleQuizSubmit = (answer, correct) => {
    setQuizFeedback(answer.toLowerCase() === correct.toLowerCase() ? "נכון!" : "לא נכון, נסה שוב.");
  };

  const handleFinalSubmit = () => {
    if (finalAnswer.length > 20) {
      setFinalFeedback("מעולה! נראה שהפנמת את החומר.");
    } else {
      setFinalFeedback("נסה להרחיב מעט – חשוב לשלב דוגמאות מהקורס.");
    }
  };

  const weeks = [
    {
      id: "week1",
      title: "שבוע 1: היכרות עם פורטוגל",
      content: (
        <>
          <ul className="list-disc pl-4 space-y-2">
            <li>סקירה היסטורית קצרה והשפעתה על התרבות</li>
            <li>ערכים מרכזיים: משפחתיות, כבוד הדדי, נינוחות</li>
            <li>השוואה תרבותית בין ישראל לפורטוגל</li>
            <li>סימולציה: איך להגיב כשמישהו מאחר לך לפגישה</li>
          </ul>
          <Video src="/videos/portugal_intro.mp4" className="mt-4 w-full" controls />
          <div className="mt-4">
            <Label htmlFor="quiz1">שאלה קצרה: מהו ערך מרכזי בתרבות הפורטוגלית?</Label>
            <Input
              id="quiz1"
              value={quizAnswer}
              onChange={(e) => setQuizAnswer(e.target.value)}
            />
            <Button onClick={() => handleQuizSubmit(quizAnswer, "משפחתיות")} className="mt-2">בדוק תשובה</Button>
            {quizFeedback && <p className="mt-2 text-sm">{quizFeedback}</p>}
          </div>
        </>
      ),
    },
    {
      id: "week2",
      title: "שבוע 2: תקשורת יומיומית",
      content: (
        <>
          <ul className="list-disc pl-4 space-y-2">
            <li>נימוסים בסיסיים בפורטוגל</li>
            <li>שפת גוף ואינטונציה</li>
            <li>סימולציה: סמול טוק בשוק המקומי</li>
          </ul>
          <Video src="/videos/communication_demo.mp4" className="mt-4 w-full" controls />
        </>
      ),
    },
    {
      id: "week3",
      title: "שבוע 3: החיים עצמם",
      content: (
        <>
          <ul className="list-disc pl-4 space-y-2">
            <li>פתיחת חשבון בנק, ביטוח לאומי ובריאות</li>
            <li>מערכת חינוך וקהילה</li>
            <li>מילון עברי-פורטוגזי שימושי</li>
          </ul>
          <Video src="/videos/life_in_portugal.mp4" className="mt-4 w-full" controls />
        </>
      ),
    },
    {
      id: "week4",
      title: "שבוע 4: משפחה, חגים ותרבות עבודה",
      content: (
        <>
          <ul className="list-disc pl-4 space-y-2">
            <li>חגים פורטוגליים ומנהגים</li>
            <li>שולחן משפחתי וערכים</li>
            <li>השתלבות במקום העבודה</li>
          </ul>
          <Video src="/videos/work_culture.mp4" className="mt-4 w-full" controls />
          <div className="mt-6">
            <Label htmlFor="final">מבחן מסכם: תאר בקצרה כיצד היית ניגש לראיון עבודה בפורטוגל.</Label>
            <Textarea
              id="final"
              rows={4}
              placeholder="כתוב כאן את התשובה שלך..."
              className="mt-2"
              value={finalAnswer}
              onChange={(e) => setFinalAnswer(e.target.value)}
            />
            <Button className="mt-2" onClick={handleFinalSubmit}>שלח תשובה</Button>
            {finalFeedback && <p className="mt-2 text-sm font-medium">{finalFeedback}</p>}
          </div>
        </>
      ),
    },
  ];

  return (
    <div className="p-6 max-w-4xl mx-auto">
      <h1 className="text-3xl font-bold mb-6 text-center">קורס התאקלמות בפורטוגל</h1>
      <Tabs defaultValue={week} onValueChange={setWeek}>
        <TabsList className="grid grid-cols-4 gap-2">
          {weeks.map((w) => (
            <TabsTrigger key={w.id} value={w.id}>
              {w.title}
            </TabsTrigger>
          ))}
        </TabsList>
        {weeks.map((w) => (
          <TabsContent key={w.id} value={w.id}>
            <Card className="mt-4">
              <CardContent className="p-4 text-right">
                {w.content}
              </CardContent>
            </Card>
          </TabsContent>
        ))}
      </Tabs>
    </div>
  );
}



