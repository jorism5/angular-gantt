# Customize

Angular Gantt is highly customizable, using either [Custom Template](#custom-template) or [Template Hooks](#template-hooks).

Angular Gantt use a template located in `src/template/default.gantt.tmpl.html`. This template is
compiled when `gantt` directive is found in your application. It contains custom `gantt-*` directives that represents
each object type you can find in the component.

## Default Template

The default template located in `src/template/default.gantt.tmpl.html` use many `gantt-*` directives that
gives a readable structure to the component. 

Lets review the major directives that compose this template.

- **gantt-labels** Left area of the component, containing rows labels and upper-left corner.

- **gantt-header** Top area of the components, with all columns headers.

- **gantt-body** Main area of the component, contains rows and columns, labels and headers excluded.

- **gantt-body-background** Background of `gantt-body`. It contains background of rows.

- **gantt-row-background** Background of a row.

- **gantt-body-foreground** Foreground of `gantt-body`. It contains the current date line.

- **gantt-body-columns** Container for all columns.

- **gantt-column** Column. It can contain timeFrames.

- **gantt-time-frame** TimeFrame.

- **gantt-body-rows** Container of all rows.

- **gantt-timespan** Timespan.

- **gantt-row** Row.

- **gantt-task** Task.

## Custom Template

You can use a custom template by copying the default template to your application and define `templateUrl`
attribute to the URL of this copy.

This is the easiest method to customize Angular Gantt, but keep in mind you will have to update your custom template
when updating Angular Gantt.

## Template Hooks

Template Hooks can be registered on any template directive.

It allows to fully customize Angular Gantt, without having to change the default template, making update process of
Angular Gantt easier than with a custom template.

Hooks can be installed using [api.directives.on.new](api.md#directives) event and uninstalled
using [api.directives.on.destroy](api.md#directives) event. Those events are raised when any template `gantt-*`
directive is added/removed from the DOM by AngularJS. They are entry points for [writing a Plugin](write_plugin.md).

    <gantt api=registerApi></gantt>

<!-- -->

    $scope.registerApi = function(api) {
    
      api.directives.on.new(scope, function(directiveName, directiveScope, directiveElement) {
        if (directiveName === 'xxxxxx') { // 'xxxxxx' is the 'gantt-*' directive name in camelCase.
          // Use directiveScope and directiveElement and do what you want.
        }
      });
      
    }

## Examples

### DOM Event Listener

Any DOM Event Listener (`click`, `dblclick`, ...) can be added on any `gantt-*` directive.

    api.directives.on.new($scope, function(directiveName, directiveScope, directiveElement) {
      if (directiveName === 'ganttTask') {
        directiveElement.bind('click', function(event) {
            event.stopPropagation();
            $log.info('task-click: ' + directiveScope.task.model);
        });
      }
    });

### Plugins

Standard plugins are good examples of what can be done using [Template Hooks](#template-hooks) and the [API](api.md). 

See sources located in `src/plugins`.