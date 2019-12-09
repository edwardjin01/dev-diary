---
title: 'Refactor payment form'
tags: ["react", ]
published: true
date: '2019-12-09'
---

Some rules to write React component:
<br>

Bad
```
<div>
  <Article></Article>
<div>
```

Good
```
<ArticleWrapper>
  <Article></Article>
</ArticleWrapper>
```
<br>
Bad

```
const premiums = _.get(plan, 'premiums', []);
const monthly = premiums.filter(item => item.mode === 'Monthly');
const annual = premiums.filter(item => item.mode === 'Annual');
const monthlyPremium = monthly && monthly.length && monthly[0].premium.totalPremium.toFixed(2);
const annualPremium = annual && annual.length && (annual[0].premium.totalPremium).toFixed(2);

const [premium, setPremium ] = useState({
  'Monthly premium': monthlyPremium
});

const [totalPayment, setTotalPayment] = useState({
  value: monthlyPremium,
})


const [planPremium, setPlanPremium] = useState(
  // eslint-disable-next-line no-nested-ternary
  !_.isEmpty(monthly) ? monthly[0] : premiums.length ? premiums[0] : {},
);

const [maxInsured, setMaxInsured] = useState({
  'maxInsured':
    plan.maxInsured || 'S$0.00'
});
```

Good
```
const premiums = _.get(plan, 'premiums', []);
const monthly = premiums.find(item => item.mode === 'Monthly');
const annual = premiums.find(item => item.mode === 'Annual');
const monthlyPremium = monthly && monthly.premium.totalPremium.toFixed(2);
const annualPremium = annual && annual.premium.totalPremium.toFixed(2);

const [premium, setPremium ] = useState({});
const [planPremium, setPlanPremium] = useState({});
const [totalPayment, setTotalPayment] = useState({})
const [maxInsured, setMaxInsured] = useState({});

useEffect(() => {
  if (annualPremium) {
    setPremium({ 'Annual premium': annualPremium });
  }
  if (monthlyPremium) {
    setPremium({ 'Monthly premium': monthlyPremium });
  }
  setPlanPremium(monthly || annual || {});
  setTotalPayment({ value: monthlyPremium || annualPremium });
  setMaxInsured({ maxInsured: plan.maxInsured || 'S$0.00'});
}, [])
```