import { useState, useEffect } from 'react';
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { format } from 'date-fns';

const generateQuestions = () => {
  const rand = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

  const questions = {
    developing: [
      [`${rand(20, 60)} + ${rand(10, 40)} =`, (a => a[0] + a[1])([rand(20, 60), rand(10, 40)])],
      [`${rand(40, 90)} − ${rand(10, 30)} =`, (a => a[0] - a[1])([rand(40, 90), rand(10, 30)])],
      [`${rand(2, 10)} × ${rand(1, 5)} =`, (a => a[0] * a[1])([rand(2, 10), rand(1, 5)])],
      [`${rand(30, 90)} ÷ 6 =`, (a => a / 6)(rand(30, 90))],
      [`1/2 + 1/4 =`, '3/4'],
      [`£1.20 + £2.35 =`, '£3.55'],
      [`300 + ? = 725`, '425'],
      [`Double 36`, '72'],
    ],
    expected: [
      [`365 + 248 =`, '613'],
      [`823 − 459 =`, '364'],
      [`24 × 6 =`, '144'],
      [`144 ÷ 12 =`, '12'],
      [`2/5 + 3/10 =`, '7/10'],
      [`£14.60 − £9.85 =`, '£4.75'],
      [`? − 267 = 389`, '656'],
      [`Half of 248`, '124'],
    ],
    greaterDepth: [
      [`4,287 + 9,654 =`, '13,941'],
      [`9,004 − 2,876 =`, '6,128'],
      [`127 × 32 =`, '4,064'],
      [`1,728 ÷ 12 =`, '144'],
      [`7/8 − 5/12 =`, '29/24 or 1 5/24`],
      [`£73.45 + £129.80 =`, '£203.25'],
      [`(13 × 12) − (7 × 9) =`, '156 − 63 = 93'],
      [`15% of 320 =`, '48'],
    ],
  };

  return questions;
};

export default function ThreeChilliDaily() {
  const [questions, setQuestions] = useState(generateQuestions());
  const [date, setDate] = useState(format(new Date(), 'do MMMM yyyy'));
  const [showAnswers, setShowAnswers] = useState(false);

  useEffect(() => {
    const savedDate = localStorage.getItem('date');
    if (savedDate !== date) {
      const newQuestions = generateQuestions();
      setQuestions(newQuestions);
      localStorage.setItem('date', date);
    }
  }, [date]);

  return (
    <div className="p-4">
      <Button className="mb-4" onClick={() => setShowAnswers(!showAnswers)}>
        {showAnswers ? 'Hide Answers' : 'Show Answers'}
      </Button>
      <main className="grid grid-cols-1 gap-4 md:grid-cols-3">
        {['developing', 'expected', 'greaterDepth'].map((level) => (
          <Card key={level}>
            <CardContent className="p-4">
              <h2 className="text-xl font-bold mb-2">
                {level === 'developing' ? 'Mild 🌶️' : level === 'expected' ? 'Hot 🌶️🌶️' : 'Spicy 🌶️🌶️🌶️'}
              </h2>
              <ol className="list-decimal pl-4 space-y-2">
                {questions[level].map((q, idx) => (
                  <li key={idx}>
                    {q[0]} {showAnswers && <span className="text-green-700 font-semibold">→ {q[1]}</span>}
                  </li>
                ))}
              </ol>
            </CardContent>
          </Card>
        ))}
      </main>
    </div>
  );
}
