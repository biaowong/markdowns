#  优选财富 CRM 系统功能说明

```php
// 其他类产品
if ($value->product_category_id ==1 ) {
  $experience[$key]->status = $product->status != 0 ?
    config('yxcrm.fixed_income.product_status')[$product->status] : '';
  if (in_array($value->trash_status, [1, 4]) && $product->status == 3) {
    $experience[$key]->is_show = 1;
  }
  else {
    $experience[$key]->is_show = 0;
  }
}
// 证券类产品
if ($value->product_category_id == 2) {
  $experience[$key]->status = $product->status != 0 ?
    config('yxcrm.portfolio_investment.product_status')[$product->status] : '';
  if (in_array($value->trash_status, [1, 4]) &&
      in_array($product->status, [3, 4, 5])) {
    $experience[$key]->is_show = 1;
  }
  else {
    $experience[$key]->is_show = 0;
  }
}
// 股权类产品
if($value->product_category_id==3){
  $experience[$key]->status = $product->status != 0 ?
    config('yxcrm.equity_investment.product_status')[$product->status] : '';
  if (in_array($value->trash_status, [1, 4]) &&
      in_array($product->status, [3, 4, 5])) {
    $experience[$key]->is_show = 1;
  }
  else {
    $experience[$key]->is_show = 0;
  }
}
// 鸿运产品
if ($value->product_category_id == config('settings.category_first')['stock']) {
  $experience[$key]->income_distribution =
    $product->income_distribution > 0 ?
    config('yxcrm.stock.income_distribution')[$product->income_distribution] : '';
  $experience[$key]->status = $product->status > 0 ?
    config('yxcrm.stock.product_status')[$product->status] : '';
  if (in_array($value->trash_status, [1]) && in_array($product->status, [2, 3, 4, 5])) {
    $experience[$key]->is_show = 1;
  }
  else {
    $experience[$key]->is_show = 0;
  }
}
```

