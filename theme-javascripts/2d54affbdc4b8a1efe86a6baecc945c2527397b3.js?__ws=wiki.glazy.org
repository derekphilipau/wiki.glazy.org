(function() {
  if ('require' in window) {
    require("discourse/lib/theme-settings-store").registerSettings(2, {"Custom_header_links":"Glazy, Glazy Recipes, https://glazy.org, vdm, self|Glazy Help, Glazy Help, https://help.glazy.org, vdm, self","links_position":"right"});
  }
})();
if ('define' in window) {
define("discourse/theme-2/initializers/theme-field-4-common-html-script-1", ["exports", "discourse/lib/plugin-api"], function (_exports, _pluginApi) {
  "use strict";

  Object.defineProperty(_exports, "__esModule", {
    value: true
  });
  _exports.default = void 0;

  var settings = require("discourse/lib/theme-settings-store").getObjectForTheme(2);

  var themePrefix = function themePrefix(key) {
    return "theme_translations.2.".concat(key);
  };

  var _default = {
    name: "theme-field-4-common-html-script-1",
    after: "inject-objects",
    initialize: function initialize() {
      (0, _pluginApi.withPluginApi)("0.8.20", function (api) {
        var customHeaderLinks = settings.Custom_header_links;
        var linksPosition = settings.links_position === "right" ? "header-buttons:before" : "home-logo:after";
        if (!customHeaderLinks.length) return;

        var h = require("virtual-dom").h;

        var headerLinks = [];
        customHeaderLinks.split("|").map(function (i) {
          var seg = $.map(i.split(","), $.trim);
          var linkText = seg[0];
          var linkTitle = seg[1];
          var linkHref = seg[2];
          var deviceClass = ".".concat(seg[3]);
          var linkTarget = seg[4] === "self" ? "" : "_blank";
          var keepOnScrollClass = seg[5] === "keep" ? ".keep" : "";
          var linkClass = ".".concat(linkText.trim().toLowerCase().replace(/\s/gi, '-'));

          if (!linkTarget) {
            headerLinks.push(h("li.headerLink".concat(deviceClass).concat(keepOnScrollClass).concat(linkClass), h("a", {
              title: linkTitle,
              href: linkHref
            }, linkText)));
          } else {
            headerLinks.push(h("li.headerLink".concat(deviceClass).concat(keepOnScrollClass).concat(linkClass), h("a", {
              title: linkTitle,
              href: linkHref,
              target: linkTarget
            }, linkText)));
          }
        });
        api.decorateWidget(linksPosition, function (helper) {
          return helper.h("ul.custom-header-links", headerLinks);
        });
        api.decorateWidget("home-logo:after", function (helper) {
          var titleVisible = helper.attrs.minimized;

          if (titleVisible) {
            $(".d-header").addClass("hide-menus");
          } else {
            $(".d-header").removeClass("hide-menus");
          }
        });
      });
    }
  };
  _exports.default = _default;
});
}
