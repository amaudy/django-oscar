==============
Oscar settings
==============

This is a comprehensive list of all the settings Oscar provides.  All settings
are optional.

Display settings
================

``OSCAR_SHOP_NAME``
-------------------

Default: ``'Oscar'``

The name of your e-commerce shop site.  This is shown as the main logo within
the default templates.

``OSCAR_SHOP_TAGLINE``
----------------------

Default: ``''``

The tagline that is displayed next to the shop name and in the browser title.

``OSCAR_PARTNER_WRAPPERS``
--------------------------

Default: ``{}``

This is an important setting which defines availability for each fulfillment
partner.  The setting should be a dict from parter name to a path to a "wrapper"
class.  For example::

    OSCAR_PARTNER_WRAPPERS = {
        'Acme': 'myproject.partners.AcmeWrapper'
        'Omnicorp': 'myproject.partners.OmnicorpWrapper'
    }

The wrapper class should subclass ``oscar.apps.partner.wrappers.DefaultWrapper``
and override the appropriate methods to control availability behaviour.

.. warning::

   This settings has been deprecated for Oscar 0.6.  Use :ref:`strategy classes <strategy_class>` 
   instead to provide availability and pricing information.

``OSCAR_RECENTLY_VIEWED_PRODUCTS``
----------------------------------

Default: 20

The number of recently viewed products to store.

``OSCAR_RECENTLY_VIEWED_COOKIE_LIFETIME``
-----------------------------------------

Default: 604800 (1 week in seconds)

The time to live for the cookie in seconds.

``OSCAR_RECENTLY_VIEWED_COOKIE_NAME``
-------------------------------------

Default: ``'oscar_history'``

The name of the cookie for showing recently viewed products.

``OSCAR_PRODUCTS_PER_PAGE``
---------------------------

Default: 20

The number of products to paginate by.

``OSCAR_SEARCH_FACETS``
------------------------------

A dictionary that specifies the facets to use with the search backend.  It
needs to be a dict with keys ``fields`` and ``queries`` for field- and
query-type facets.  The default is::

    OSCAR_SEARCH_FACETS = {
        'fields': {
            # The key for these dicts will be used when passing facet data
            # to the template. Same for the 'queries' dict below.
            'category': {
                'name': _('Category'),
                'field': 'category'
            }
        },
        'queries': {
            'price_range': {
                'name': _('Price range'),
                'field': 'price',
                'queries': [
                    # This is a list of (name, query) tuples where the name will
                    # be displayed on the front-end.
                    (_('0 to 40'), '[0 TO 20]'),
                    (_('20 to 40'), '[20 TO 40]'),
                    (_('40 to 60'), '[40 TO 60]'),
                    (_('60+'), '[60 TO *]'),
                ]
            }
        }
    }


``OSCAR_PROMOTION_POSITIONS``
-----------------------------

Default::

    OSCAR_PROMOTION_POSITIONS = (('page', 'Page'),
                                 ('right', 'Right-hand sidebar'),
                                 ('left', 'Left-hand sidebar'))

The choice of display locations available when editing a promotion. Only 
useful when using a new set of templates.

``OSCAR_PROMOTION_MERCHANDISING_BLOCK_TYPES``
---------------------------------------------

Default::

    COUNTDOWN, LIST, SINGLE_PRODUCT, TABBED_BLOCK = (
        'Countdown', 'List', 'SingleProduct', 'TabbedBlock')
    OSCAR_PROMOTION_MERCHANDISING_BLOCK_TYPES = (
        (COUNTDOWN, "Vertical list"),
        (LIST, "Horizontal list"),
        (TABBED_BLOCK, "Tabbed block"),
        (SINGLE_PRODUCT, "Single product"),
    )

Defines the available promotion block types that can be used in Oscar.

.. _OSCAR_DASHBOARD_NAVIGATION:

``OSCAR_DASHBOARD_NAVIGATION``
------------------------------

Default: see ``oscar.defaults`` (too long to include here).

A list of dashboard navigation elements. Usage is explained in
:doc:`/howto/how_to_configure_the_dashboard_navigation`.

Order settings
==============

``OSCAR_INITIAL_ORDER_STATUS``
------------------------------

The initial status used when a new order is submitted. This has to be a status
that is defined in the ``OSCAR_ORDER_STATUS_PIPELINE``.

``OSCAR_INITIAL_LINE_STATUS``
-----------------------------

The status assigned to a line item when it is created as part of an new order. It
has to be a status defined in ``OSCAR_ORDER_STATUS_PIPELINE``.

``OSCAR_ORDER_STATUS_PIPELINE``
-------------------------------

Default: ``{}``

The pipeline defines the statuses that an order or line item can have and what
transitions are allowed in any given status. The pipeline is defined as a
dictionary where the keys are the available statuses. Allowed transitions are
defined as iterable values for the corresponding status. 

A sample pipeline (as used in the Oscar sandbox) might look like this::

    OSCAR_INITIAL_ORDER_STATUS = 'Pending'
    OSCAR_INITIAL_LINE_STATUS = 'Pending'
    OSCAR_ORDER_STATUS_PIPELINE = {
        'Pending': ('Being processed', 'Cancelled',),
        'Being processed': ('Processed', 'Cancelled',),
        'Cancelled': (),
    }

``OSCAR_ORDER_STATUS_CASCADE``
------------------------------

This defines a mapping of status changes for order lines which 'cascade' down
from an order status change.

For example::

    OSCAR_ORDER_STATUS_CASCADE = {
        'Being processed': 'In progress'
    }

With this mapping, when an order has it's status set to 'Being processed', all
lines within it have their status set to 'In progress'.  In a sense, the status
change cascades down to the related objects.

Note that this cascade ignores restrictions from the
``OSCAR_LINE_STATUS_PIPELINE``.

``OSCAR_LINE_STATUS_PIPELINE``
------------------------------

Default: ``{}``

Same as ``OSCAR_ORDER_STATUS_PIPELINE`` but for lines.

Checkout settings
=================

``OSCAR_ALLOW_ANON_CHECKOUT``
-----------------------------

Default: ``False``

Specifies if an anonymous user can buy products without creating an account
first.  If set to ``False`` users are required to authenticate before they can
checkout (using Oscar's default checkout views).

``OSCAR_REQUIRED_ADDRESS_FIELDS``
---------------------------------

Default: ``('first_name', 'last_name', 'line1', 'city', 'postcode', 'country')``

List of form fields that a user has to fill out to validate an address field.

Review settings
===============

``OSCAR_ALLOW_ANON_REVIEWS``
----------------------------

Default: ``True``

This setting defines whether an anonymous user can create a review for
a product without registering first. If it is set to ``True`` anonymous
users can create product reviews.

``OSCAR_MODERATE_REVIEWS``
--------------------------

Default: ``False``

This defines whether reviews have to be moderated before they are publicly
available. If set to ``False`` a review created by a customer is immediately
visible on the product page.

Communication settings
======================

``OSCAR_EAGER_ALERTS``
----------------------

Default: ``True``

This enables sending alert notifications/emails instantly when products get
back in stock by listening to stock record update signals this might impact
performance for large numbers stock record updates.
Alternatively, the management command ``oscar_send_alerts`` can be used to
run periodically, e.g. as a cronjob. In this case instant alerts should be
disabled.

``OSCAR_SEND_REGISTRATION_EMAIL``
---------------------------------

Default: ``True``

Sending out *welcome* messages to a user after they have registered on the
site can be enabled or disabled using this setting. Setting it to ``True``
will send out emails on registration.

``OSCAR_FROM_EMAIL``
--------------------

Default: ``oscar@example.com``

The email address used as the sender for all communication events and emails
handled by Oscar.

``OSCAR_STATIC_BASE_URL``
-------------------------

Default: ``None``

A URL which is passed into the templates for communication events.  It is not
used in Oscar's default templates but could be used to include static assets
(eg images) in a HTML email template.

Offer settings
==============

``OSCAR_OFFER_BLACKLIST_PRODUCT``
---------------------------------

Default: ``None``

A function which takes a product as its sole parameter and returns a boolean
indicating if the product is blacklisted from offers or not.

Example::

    from decimal import Decimal as D

    def is_expensive(product):
        if product.has_stockrecord:
            return product.stockrecord.price_incl_tax < D('1000.00')
        return False

    # Don't allow expensive products to be in offers
    OSCAR_OFFER_BLACKLIST_PRODUCT = is_expensive

``OSCAR_OFFER_ROUNDING_FUNCTION``
---------------------------------

Default: Round down to the nearest hundredth of a unit using ``decimal.Decimal.quantize``

A function responsible for rounding decimal amounts when offer discount
calculations don't lead to legitimate currency values.

Basket settings
===============

``OSCAR_BASKET_COOKIE_LIFETIME``
--------------------------------

Default: 604800 (1 week in seconds)

The time to live for the basket cookie in seconds.

``OSCAR_MAX_BASKET_QUANTITY_THRESHOLD``
---------------------------------------

Default: ``None``

The maximum number of products that can be added to a basket at once.

``OSCAR_BASKET_COOKIE_OPEN``
----------------------------

Default: ``'oscar_open_basket'``

The name of the cookie for the open basket.

``OSCAR_BASKET_COOKIE_SAVED``
-----------------------------

Default: ``'oscar_saved_basket'``

The name of the cookie for the saved basket.

Currency settings
=================

``OSCAR_DEFAULT_CURRENCY``
--------------------------

Default: ``GBP``

This should be the symbol of the currency you wish Oscar to use by default.
This will be used by the currency templatetag.

``OSCAR_CURRENCY_LOCALE``
-------------------------

Default: ``None``

This can be used to customise currency formatting. The value will be passed to
the ``format_currency`` function from the `Babel library`_.

.. _`Babel library`: http://babel.edgewall.org/wiki/ApiDocs/0.9/babel.numbers#babel.numbers:format_decimal

``OSCAR_CURRENCY_FORMAT``
-------------------------

Default: ``None``

This can be used to customise currency formatting. The value will be passed to
the ``format_currency`` function from the Babel library.

Upload/media settings
=====================

``OSCAR_IMAGE_FOLDER``
----------------------

Default: ``images/products/%Y/%m/``

The location within the ``MEDIA_ROOT`` folder that is used to store product images.
The folder name can contain date format strings as described in the `Django Docs`_.

.. _`Django Docs`: https://docs.djangoproject.com/en/dev/ref/models/fields/#filefield


``OSCAR_PROMOTION_FOLDER``
--------------------------

Default: ``images/promotions/``

The folder within ``MEDIA_ROOT`` used for uploaded promotion images.

``OSCAR_MISSING_IMAGE_URL``
---------------------------

Default: ``image_not_found.jpg``

Copy this image from ``oscar/static/img`` to your ``MEDIA_ROOT`` folder. It needs to
be there so Sorl can resize it.

``OSCAR_UPLOAD_ROOT``
---------------------

Default: ``/tmp``

The folder is used to temporarily hold uploaded files until they are processed.
Such files should always be deleted afterwards.

Slug settings
=============

``OSCAR_SLUG_MAP``
------------------

Default: ``None``

A dictionary to map strings to more readable versions for including in URL
slugs.  This mapping is appled before the slugify function.  
This is useful when names contain characters which would normally be
stripped.  For instance::

    OSCAR_SLUG_MAP = {
        'c++': 'cpp',
        'f#': 'fshared',
    }

``OSCAR_SLUG_FUNCTION``
-----------------------

Default: ``django.template.defaultfilters.slugify``

The slugify function to use.  Note that is used within Oscar's slugify wrapper
(in ``oscar.core.utils``) which applies the custom map and blacklist.

Example::

    def some_slugify(value)
        pass

    OSCAR_SLUG_FUNCTION = some_slugify

``OSCAR_SLUG_BLACKLIST``
------------------------

Default: ``None``

A list of words to exclude from slugs.

Example::

    OSCAR_SLUG_BLACKLIST = ['the', 'a', 'but']

Misc settings
=============

``OSCAR_COOKIES_DELETE_ON_LOGOUT``
----------------------------------

Default: ``['oscar_recently_viewed_products',]``

Which cookies to delete automatically when the user logs out.
