---
category: Components
type: Data Entry
cols: 1
title: Form
---

Form is used to collect, validate, and submit the user input, usually contains various form items including checkbox, radio, input, select, and etc.

## Form

You can align the controls of a `form` using one of the following attributes：

- `horizontal`：to horizontally align the `label`s and controls of the fields.
- `inline`：to render the labels and controls of the fields in one line (by setting the display property of form controls to `inline-block`).

## Form fields

A form consists of one or more form fields whose type includes input, textarea, checkbox, radio, select, tag, and more.
A form field is defined using `<Form.Item />`.

```jsx
<Form.Item {...props}>
  {children}
</Form.Item>
```

> PS：By default, large size controls are used within a form.

## API

### Form

**more example [rc-form](http://react-component.github.io/form/)**。

| Property  | Description                              | Type       | Default Value |
|-----------|------------------------------------------|------------|---------|
| form | Decorated by `Form.create()` will be automatically set `this.props.form` property, so just pass to form, you don't need to set it by yourself after 1.7.0. | object | n/a
| vertical | Use vertical layout. | boolean | false |
| horizontal | Use horizontal layout. | boolean  | false    |
| inline | Use inline alignment. | boolean | false |
| onSubmit | Defines a function will be called if form data validation is successful. | Function(e:Event) |  |

### Form.create(options)

How to use：

```jsx
class CustomizedForm extends React.Component {}

CustomizedForm = Form.create({})(CustomizedForm);
```

The following `options` are available:

| Property      | Description                          | Type       |
|-----------|------------------------------------------|------------|
| onFieldsChange | Specify a function that will be called when the value a `Form.Item` gets changed. Usage example: saving the field's value to Redux store. | Function(props, fields) |
| mapPropsToFields | Convert props to corresponding field value. Usage example: reading the values from Redux store. | Function(props): Object{ fieldName: Object{ value } } |

If the form has been decorated by `Form.create` then it has `this.props.form` property. `this.props.form` provides some APIs as follows:

| Property      | Description                          | Type       |
|-----------|------------------------------------------|------------|
| getFieldsValue | Get the specified fields' values. If you don't specify a parameter, you will get all fields' values. | Function([fieldNames: string[]]) |
| getFieldValue | Get the value of a field. | Function(fieldName: string) |
| setFieldsValue | Set the value of a field.(Note: please don't use it in `componentWillReceiveProps`, otherwise, it will cause an endless loop, [more](https://github.com/ant-design/ant-design/issues/2985)) | Function({ [fieldName]: value } |
| setFields | Set the value and error of a field. | Function(obj: object) |
| validateFields | Validate the specified fields and get theirs values and errors. | Function([fieldNames: string[]], [options: object], callback: Function(errors, values)) |
| validateFieldsAndScroll | This function is similar to `validateFields`, but after validation, if the target field is not in visible area of form, form will be automatically scrolled to the target field area. | same as `validateFields` |
| getFieldError | Get the error of a field. | Function(name) |
| isFieldValidating | Check if the specified field is being validated. | Function(name) |
| resetFields | Reset the specified fields' value(to `initialValue`) and status. If you don't specify a parameter, all the fields will be reset. | Function([names: string[]]) |
| getFieldDecorator | Two-way binding for form, please read below for details. | |

### this.props.form.getFieldDecorator(id, options)

After wrapped by `getFieldDecorator`, `value`(or other property defined by `valuePropName`) `onChange`(or other property defined by `trigger`) props will be added to form controls，the flow of form data will be handled by Form which will cause:

1. You don't need to use `onChange` to collect data, but you still can listen to `onChange`(and so on) events.
2. You can not set value of form control via `value` `defaultValue` prop, and you should set default value with `initialValue` in `getFieldDecorator` instead.
3. You don't need to call `setState` manually, please use `this.props.form.setFieldsValue` to change value programmatically.

#### Special attention

If you use `react@<15.3.0`, then, you can't use `getFieldDecorator` in stateless component: https://github.com/facebook/react/pull/6534

#### getFieldDecorator's parameters

| Property  | Description                             | Type | Default Value |
|-----------|-----------------------------------------|------|---------------|
| options.id | The unique identifier is required. | string | |
| options.valuePropName | Props of children node, for example, the prop of Switch is 'checked'. | string | 'value' |
| options.initialValue | You can specify initial value, type, optional value of children node. | | n/a |
| options.trigger | When to collect the value of children node | string | 'onChange' |
| options.getValueFromEvent | To convert parameters of onChange to the value of control, for example, set value of DatePicker: `(date, dateString) => dateString` | function(..args) | [reference](https://github.com/react-component/form#optiongetvaluefromevent) |
| options.validateTrigger | When to validate the value of children node. | string | 'onChange' |
| options.rules | Includes validation rules. Please refer to [async-validator](https://github.com/yiminghe/async-validator) for details. | array | n/a |
| options.exclusive | Whether it is exclusive with other controls, particularly for Radio. | boolean | false |

### Form.Item

Note:
* If Form.Item has multiple children that had been decorated by `getFieldDecorator`, `help` and `required` and `validateStatus` can't be generated automatically.
* Before `2.2.0`, form controls must be child of Form.Item, otherwise, you need to set `help`, `required` and `validateStatus` by yourself.

| Property      | Description                          | Type   | Default Value |
|---------------|--------------------------------------|--------|---------------|
| label | Label text | string  |   |
| labelCol | The layout of label. You can set `span` `offset` to something like `{span: 3, offset: 12}` same as with `<Col>` | object |  |
| wrapperCol | The layout for input controls. Same as `labelCol` | object |  |
| help | The prompt message. If not provided, the prompt message will be generated by the validation rule. | string |  |
| extra | The extra prompt message. It is similar to help. Usage example: to display error message and prompt message at the same time. | string |  |
| required | Whether provided or not, it will be generated by the validation rule. |  boolean | false |
| validateStatus | The validation status. If not provided, it will be generated by validation rule. options: 'success' 'warning' 'error' 'validating' | string |  |
| hasFeedback | Used with `validateStatus`, this option specifies the validation status icon. Recommended to be used only with `Input`. | boolean | false |

<style>
.code-box-demo .ant-form-horizontal {
  max-width: 540px;
}
</style>
