## Purpose
This capability calculates the final pricing for an order, applying tier-based and coupon-based discounts sequentially while preventing negative totals and ensuring correct rounding.

## ADDED Requirements

### Requirement: Tier discounts
The system SHALL apply a percentage discount based on the customer's loyalty tier. Gold tier receives 10%, Silver receives 5%, others 0%.

#### Scenario: Gold tier discount
- **WHEN** the order belongs to a gold tier customer
- **THEN** a 10% discount is applied to the subtotal of the items

#### Scenario: Rounding on half kopeck
- **WHEN** the discount calculation results in a half kopeck
- **THEN** it rounds to the nearest whole kopeck using standard rounding

### Requirement: Coupon discounts
The system SHALL apply valid coupons in the order they are provided, calculated from the remaining subtotal after the tier discount.

#### Scenario: Multiple sequential coupons
- **WHEN** multiple coupons are applied
- **THEN** each coupon reduces the remaining balance sequentially

#### Scenario: Min subtotal threshold
- **WHEN** a coupon specifies a minimum subtotal
- **THEN** it checks the original subtotal of the order (before any discounts) to determine validity

### Requirement: Category specific coupons
The system SHALL apply category-specific coupons only to the original value of items matching that category.

#### Scenario: Category coupon calculation
- **WHEN** a category-specific coupon is provided
- **THEN** the discount is calculated based on the original subtotal of items in that category

### Requirement: Negative total prevention
The system SHALL ensure the subtotal never drops below 0 kopecks after discounts.

#### Scenario: Discount exceeds subtotal
- **WHEN** the total discount applied exceeds the subtotal
- **THEN** the total discount is capped at the subtotal, leaving 0 to pay (plus shipping)

### Requirement: Invalid coupon handling
The system SHALL ignore expired or invalid coupons silently.

#### Scenario: Expired coupon
- **WHEN** an expired coupon is provided
- **THEN** it is ignored and contributes 0 to the discount
