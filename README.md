function params = hw2trainbnb(X,Y)
    class_prior_number  = unique(Y)
    trainSize = length(Y);
    dim = size(X)
    dim = dim(2)
    class_prior = zeros(1,class_prior_number);
    for i = 1 : class_prior_number
        class_prior(i) = sum(Y==i)/trainSize;
    end
    
    MLE = zeros(class_prior_number,dim);
    for i = 1 : class_prior_number
        temp_array = data(labels==i,:);
        class_size = size(temp_array,1)
        jthEntry = sum(temp_array,1)';
        MLE(i,:) = (1+ jthEntry)/(2+class_size)
    end
    params.mle = MLE;
    params.prior = class_prior;
end


function preds = hw2TestBnb(params,test)
test = full(test);
row_dim = size(test,1);
class_size = length(params.prior);
labels = zeros(1,row_dim);
for i = 1:row_dim
    max = -1;
    class_index = -1;
    
    for j = 1 : class_size
        temp_array_xj = test(i,:);
        temp_array_prob = params.mle(j,:);
        product_first = temp_array_prob.^temp_array_xj;
        product_second = (1-temp_array_prob).^(1-temp_array_xj);
        productTotal = prod(product_first)*prod(product_second)*params.prior(j);
        if(productTotal>max)
            max = productTotal;
            class_index = j;
        end
    end
    labels(1,i) = class_index;
end
preds = labels;
end


params = hw2trainbnb(data,labels);
preds = hw2TestBnb(params,data);


