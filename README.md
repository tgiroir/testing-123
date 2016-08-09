# testing-123
{% layout settings.customer_layout %}

<div class="section-title desktop-12 mobile-3">
  <h1>{{ 'customer.order.title' | t }} {{ order.name }}</h1>
</div>

<div id="customer-wrapper" class="desktop-12 mobile-3">


  {% if order.cancelled %}
  <div id="order_cancelled" class="flash notice">
    <h5 id="order_cancelled_title">{{ 'customer.order.cancelled' | t }} <span class="note">{{ order.cancelled_at | date: "%B %d, %Y %I:%M%p" }}</span></h5>
    <span class="note">{{ 'customer.order.cancelled_reason' | t }}</span>
  </div>
  {% endif %}

  <div class="note order_date">{{ 'customer.order.date' | t }} {{ order.created_at | date: "%B %d, %Y %I:%M%p" }}</div>

  <div id="order_address" class="group">
    
    <div id="order_payment" class="desktop-6 table-3 mobile-3">
      <h5 class="order_section_title">{{ 'customer.order.billing_address' | t }}</h5>
      <p><span class="note">{{ 'customer.order.payment_status' | t }}:</span> <span class="status_{{ order.financial_status }}">{{ order.financial_status_label }}</span></p>
      
      <div class="address note">
        <p>{{ order.billing_address.name }}</p>
        <p>{{ order.billing_address.company }}</p>
        <p>{{ order.billing_address.street }}</p>
        <p>{{ order.billing_address.city }}, {{ order.billing_address.province }}</p>
        <p>{{ order.billing_address.country }} {{ order.billing_address.zip }}</p>
        <p>{{ order.billing_address.phone }}</p>
      </div>
      
    </div>
    
    
    {% if order.shipping_address %}
    <div id="order_shipping" class="desktop-6 table-3 mobile-3">      
      <h5 class="order_section_title">{{ 'customer.order.shipping_address' | t }}</h5>
      <p><span class="note">{{ 'customer.order.fulfillment_status' | t }}:</span> <span class="status_{{ order.fulfillment_status }}">{{ order.fulfillment_status_label }}</span></p>
      
      <div class="address note">
        <p>{{ order.shipping_address.name }}</p>
        <p>{{ order.shipping_address.company }}</p>
        <p>{{ order.shipping_address.street }}</p>
        <p>{{ order.shipping_address.city }}, {{ order.shipping_address.province }}</p>
        <p>{{ order.shipping_address.country }} {{ order.shipping_address.zip }}</p>
        <p>{{ order.shipping_address.phone }}</p>
      </div>
      
    </div>
    {% endif %}
    
  </div>

  <table id="order_details">
    <thead>
      <tr>
        <th>{{ 'customer.order.details.product' | t }}</th>
        <th>{{ 'customer.order.details.sku' | t }}</th>
        <th>{{ 'customer.order.details.size' | t }}</th>
        <th>{{ 'customer.order.details.length' | t }}</th>
        <th>{{ 'customer.order.details.price' | t }}</th>
        <th class="center">{{ 'customer.order.details.quantity' | t }}</th>
        <th class="center">{{ 'customer.order.details.total' | t }}</th>
      </tr>
    </thead>
    <tbody>
      {% for line_item in order.line_items %}
      
      <tr id="{{ line_item.id }}" class="{% cycle 'odd', 'even' %}">
        <td class="product">
          {{ line_item.title | link_to: line_item.product.url }} 
        
          {% if line_item.fulfillment %}
          <div class="note">
            Fulfilled {{ line_item.fulfillment.created_at | date: "%b %d" }}
            {% if line_item.fulfillment.tracking_number %}
            <a href="{{ line_item.fulfillment.tracking_url }}">{{ line_item.fulfillment.tracking_company }} #{{ line_item.fulfillment.tracking_number}}</a>
            {% endif %}
          </div>
          {% endif %}
        </td>
        <td class="sku note">{{ line_item.sku }}</td>
        <td class="size">{{ line_item.size}}</td>
        <td class="length">{{ line_item.length}}</td>
        <td class="money">{{ line_item.price | money }}</td>
        <td class="quantity center">{{ line_item.quantity }}</td>
        <td class="total money center">{{ line_item.quantity | times: line_item.price | money }}</td>
      </tr>
      {% endfor %}
    </tbody>  
    <tfoot>
      <tr class="order_summary note">
        <td class="label" colspan="4">{{ 'customer.order.details.subtotal' | t }}</td>
        <td class="total money center">{{ order.subtotal_price | money }}</td>
      </tr>   

      {% for discount in order.discounts %}
      <tr class="order_summary discount">
        <td class="label" colspan="4">{{ discount.code }} {{ 'customer.order.discount' | t }}</td>
        <td class="total money center">{{ discount.savings | money }}</td>
      </tr>
      {% endfor %}

      {% for shipping_method in order.shipping_methods %}
      <tr class="order_summary note">
        <td class="label" colspan="4">{{ 'customer.order.shipping' | t }} ({{ shipping_method.title }}):</td>
        <td class="total money center">{{ shipping_method.price | money }}</td>
      </tr>
      {% endfor %}

      {% for tax_line in order.tax_lines %}
      <tr class="order_summary note">
        <td class="label" colspan="4">{{ 'customer.order.tax' | t }} ({{ tax_line.title }} {{ tax_line.rate | times: 100 }}%):</td>
        <td class="total money center">{{ tax_line.price | money }}</td>
      </tr>
      {% endfor %}    

      <tr class="order_summary order_total">
        <td class="label" colspan="4">{{ 'customer.order.details.total' | t }}:</td>
        <td class="total money center">{{ order.total_price | money }} {{ order.currency }}</td>
      </tr>   
    </tfoot>  
  </table>

</div>
